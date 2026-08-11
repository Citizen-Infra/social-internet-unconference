---
shaping: true
---

# Spike: Jitsi Scheduling, Identity, Attendance, and Recording

**Status:** Complete; JaaS pilot selected
**Date:** 2026-08-11

## Context

The selected Unconference shape asks Avails to create a Jitsi room when an
organizer promotes a ready topic. CA then needs trusted attendance data for
private transcript access, and the SIU recording connector needs a supported way
to capture the room. The repository README currently proposes MiroTalk SFU for
the synchronous main event.

## Goal

Identify the concrete conferencing, identity, attendance, and recording steps
needed to take one promoted CA topic from an Avails booking to a recorded room
with a trustworthy participant list.

## Decision

Use 8x8 Jitsi as a Service (JaaS) for the first scheduled-session pilot. This is
not a decision to replace MiroTalk as the main-event lobby or room stack. It is
the smallest verified mechanism for authenticated Jitsi participants,
join/leave events, hosted file recording, and a downloadable recording event.

Treat JaaS as replaceable infrastructure behind CA's join and webhook seams.
The pilot sends meeting media through 8x8 temporarily. SIU must revisit
self-hosted Jitsi plus Jibri before claiming a fully community-hosted media path.

## Verified Mechanisms

| Concern | JaaS mechanism | Consequence |
|---|---|---|
| Room allocation | A room is an opaque name under the JaaS AppID; `ROOM_CREATED` occurs when the first participant joins. | Avails generates a random room name. There is no pre-creation API call to retry. |
| Authentication | Every endpoint receives an RS256 JWT with a literal `room`, AppID `sub`, `context.user.id`, `nbf`, and `exp`. | CA owns the private signing key and issues tokens only at join time. |
| Stable identity | Participant webhook `data.id` reflects the unique user identifier supplied in the JWT. | CA uses its immutable account ID, not email, as the attendance key. |
| Authorization | JWT `context.user.moderator` controls moderator status; `context.features.recording` controls recording permission. | Only organizer tokens get moderator and recording permissions. |
| Attendance | `PARTICIPANT_JOINED` and `PARTICIPANT_LEFT` include `sessionId`, room FQN, timestamp, participant ID, and JWT user ID. | CA can add authenticated attendees to the session ACL and retain join/leave evidence. |
| Delivery safety | Webhooks carry `idempotencyKey`; failed requests retry with exponential backoff and ordering is not guaranteed. | CA deduplicates before processing and never depends on arrival order. |
| Webhook authentication | JaaS can send a configured `Authorization` header. | Use a dedicated high-entropy bearer secret, validate AppID and room FQN, and rotate it independently of JWT keys. |
| Recording | A permitted moderator can initiate file recording, including through the IFrame API. | The CA room wrapper starts recording only after the organizer explicitly confirms it. |
| Media handoff | `RECORDING_UPLOADED` includes a pre-authenticated download URL, participants, initiator, duration, timestamps, and recording session ID. | An SIU ingestion worker downloads and verifies the file immediately. |
| Retention | JaaS recordings and their download links last 24 hours; one recording session is limited to six hours. | Never treat JaaS as the archive. Alert well before expiry and split unusually long sessions. |

## Scheduling Contract

CA calls the existing Avails `schedule_call` operation with its current
participants, candidate windows, and idempotency key plus:

```json
{
  "conference": {
    "provider": "jaas",
    "join_base_url": "https://<ca-host>/api/events/join"
  }
}
```

Avails selects the slot, generates at least 128 bits of cryptographic randomness
for `room_name`, and stores the following in the existing booking ledger:

```json
{
  "conference": {
    "provider": "jaas",
    "room_name": "<opaque-random-name>",
    "join_url": "https://<ca-host>/api/events/join/<booking-id>"
  }
}
```

The same Avails idempotency key must return the same slot, booking ID, room name,
and join URL. Avails puts the CA join URL in ICS, not an `8x8.vc` URL or JWT.
JaaS AppID, API key ID, and private key remain CA integration configuration and
never enter Avails.

## Join Contract

`GET /api/events/join/:bookingId` is a CA-hosted room wrapper:

1. Require an authenticated account and current membership in the event's
   community. Apply the event's invitation policy before allowing entry.
2. Show the recording notice and require an explicit acknowledgement for this
   session. Record policy version, account ID, and timestamp.
3. Issue a short-lived, literal-room JWT server-side. Set `context.user.id` to
   the immutable CA account ID. Set moderator and recording permissions only for
   organizers. Do not expose the signing key or persist the JWT.
4. Embed `8x8.vc` with room name `<AppID>/<room_name>` and the JWT. Do not put the
   JWT in URLs, ICS, Google Calendar, logs, or the brain.

The token header uses `alg: RS256`, the configured API-key `kid`, and
`typ: JWT`. Its body is room-specific rather than the wildcard shown in many
JaaS examples:

```json
{
  "aud": "jitsi",
  "iss": "chat",
  "sub": "<AppID>",
  "room": "<room_name>",
  "nbf": "<now-minus-clock-skew>",
  "exp": "<short-expiry>",
  "context": {
    "room": { "regex": false },
    "user": {
      "id": "<ca-account-id>",
      "name": "<display-name>",
      "email": "<verified-email>",
      "moderator": false
    },
    "features": { "recording": false }
  }
}
```

The symbolic `nbf` and `exp` values above are encoded as NumericDate integers.
For an organizer, CA sets both `moderator` and `recording` to `true`.

The join route opens shortly before the event. Tokens use a small clock-skew
allowance and expire quickly enough that cancellation can take effect. A
cancelled event stops issuing tokens immediately; already issued tokens age out.

## Attendance Contract

JaaS sends `ROOM_CREATED`, `PARTICIPANT_JOINED`, `PARTICIPANT_LEFT`, and
`ROOM_DESTROYED` to a CA webhook endpoint configured with a dedicated
`Authorization` header. CA:

- Rejects the wrong authorization value, AppID, or room FQN.
- Inserts the JaaS `idempotencyKey` before applying an event and returns `2xx`
  for an already processed key.
- Maps webhook `data.id` to the CA account ID previously placed in the JWT.
- Adds a participant to the transcript ACL only after `PARTICIPANT_JOINED`.
- Stores timestamps as evidence but computes presence without assuming webhook
  order. A missing leave event does not erase a verified join.
- Does not admit anonymous JaaS participants in the pilot.

## Recording And Handoff

The CA room wrapper gives only an organizer a JWT with moderator and recording
permissions. After the organizer confirms that recording should start, the
wrapper invokes IFrame `startRecording` in `file` mode. JaaS supplies its own
recording indicator; CA's acknowledgement remains the durable consent record.

```javascript
api.executeCommand('startRecording', { mode: 'file' })
```

CA processes `RECORDING_STARTED`, `RECORDING_ENDED`, and
`RECORDING_UPLOADED`. On upload it enqueues an idempotent SIU ingestion job keyed
by the JaaS recording session ID. The worker must download the pre-authenticated
URL immediately, verify a non-empty supported media type, calculate a SHA-256
hash, store the media in private SIU storage, and report that artifact to CA.
The transcription-provider path starts at that private artifact; no
transcription provider joins JaaS or owns attendance.

If ingestion fails, retry from the same event payload with exponential backoff
and alert before the 24-hour URL expiry. Duplicate upload events must resolve to
the same artifact. The JaaS URL is sensitive and must not appear in normal logs.

## Updates And Cancellation

- A time or title update keeps the Avails booking ID and JaaS room name, updates
  CA/My Community and Google Calendar, and reissues ICS as needed.
- A participant update changes invitations and CA authorization; it does not
  alter the room name.
- Cancellation marks the Avails booking and CA event cancelled, updates the
  shared calendar, stops new join tokens, and cancels pending recording work.
- JaaS has no room resource to delete before first join. A cancelled room name
  simply remains unreachable through CA and expires as a secret identifier.
- Once recording has begun, cancellation does not delete captured media; the
  community retention policy governs deletion.

## Residual Pilot Checks

These are production verification, not unresolved architecture:

- Confirm the selected JaaS plan enables file recording and all required
  webhooks in the intended data region.
- Confirm `data.id` round-trips the exact CA account ID for join and leave.
- Confirm the IFrame file-recording command produces `RECORDING_UPLOADED`
  without Dropbox configuration on JaaS.
- Measure upload latency and verify the ingestion alert leaves enough margin
  before the 24-hour expiry.
- Run cancellation and out-of-order webhook tests.

## Acceptance Result

The spike establishes concrete booking, credential, attendance, recording,
retry, media handoff, update, and cancellation mechanisms. It closes the Jitsi
portion of R4. R5 remains open because the pilot's recorder is hosted by 8x8 and
its selected transcription provider, Deepgram, is also hosted rather than on
community infrastructure.

## Sources

- [JaaS IFrame integration](https://developer.8x8.com/jaas/docs/iframe-api-integration/)
- [JaaS JWT claims](https://developer.8x8.com/jaas/docs/api-keys-jwt/)
- [JaaS webhook setup and delivery](https://developer.8x8.com/jaas/docs/jaas-console-webhooks/)
- [JaaS webhook events](https://developer.8x8.com/jaas/docs/webhooks-events/)
- [JaaS webhook payloads](https://developer.8x8.com/jaas/docs/webhooks-payload/)
- [JaaS recording and retention](https://developer.8x8.com/jaas/docs/jaas-prefs-recording/)
- [JaaS recording FAQ](https://developer.8x8.com/jaas/docs/faq/#how-does-recording-meetings-work-recording--meeting-recording)
- [Jitsi IFrame commands](https://jitsi.github.io/handbook/docs/dev-guide/dev-guide-iframe-commands/#startrecording)
