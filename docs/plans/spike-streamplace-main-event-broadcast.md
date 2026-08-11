---
shaping: true
---

# Spike: Streamplace Main-Event Broadcast

**Status:** Candidate contract captured; end-to-end integration not selected
**Date:** 2026-08-11

## Role

Streamplace is a candidate public broadcast and discovery layer for the
synchronous main event. It is not the room system, lobby, attendance authority,
private recording store, or transcript store.

This workstream is separate from V1-V8. Those slices schedule and preserve
smaller sessions through JaaS and the private SIU artifact pipeline. The
main-event room and broadcast decision depends on event format, scale, and
consent policy.

## Verified Hosted Path

- A broadcaster signs in with an AT Protocol account through OAuth.
- Streamplace accepts H.264 video over RTMPS or WHIP. RTMPS uses AAC audio;
  WHIP uses Opus in the documented OBS setup.
- The broadcaster generates a stream key or bearer token in the Live Dashboard.
- The broadcaster explicitly announces the livestream with its title, optional
  thumbnail, and content warnings. The resulting stream is public.
- Streamplace can forward an incoming stream to as many as five active RTMP or
  RTMPS multistream targets.

## Missing MiroTalk Bridge

MiroTalk's RTMP documentation describes operating Node Media Server or Nginx
RTMP and publishing with OBS. It does not document composing a live MiroTalk
room and pushing that composition to an arbitrary external RTMPS or WHIP ingest
endpoint.

Therefore the earlier statement that a direct MiroTalk-to-Streamplace path
partly exists is not an established mechanism. The pilot needs one of:

1. A human-operated OBS scene that captures the selected MiroTalk room.
2. A dedicated server-side compositor that consumes room media and publishes
   H.264/AAC over RTMPS or H.264/Opus over WHIP.
3. A separately verified MiroTalk feature or extension that provides equivalent
   composed outbound media.

The human-operated OBS path is acceptable only for a rehearsal. It depends on
one operator device, can accidentally expose notifications or private windows,
and provides weak recovery if that device or network fails.

## Identity And Secrets

- Use an SIU-controlled AT Protocol account for the event broadcaster. Do not
  couple the event stream to an organizer's personal account.
- Keep the RTMP stream key or WHIP bearer token only in the broadcaster or
  compositor secret store. It must not enter CA, calendars, logs, URLs in the
  brain, or source control.
- Publish only the stable public playback URL through CA, My Community, the
  shared calendar, and approved brain records.
- Streamplace identity authenticates the broadcaster. It does not establish
  room attendance or authorize access to private artifacts.

## Consent Boundary

The Streamplace hosted flow is public by default. Only a room explicitly marked
for public broadcast may feed the compositor. Private scheduled sessions and
their JaaS recording/transcription pipeline remain disconnected from
Streamplace unless organizers and participants approve a separate public
broadcast action.

The live room must show a persistent public-broadcast indicator. Starting or
changing the broadcast requires an organizer action; repository edits and
scheduling events cannot trigger it.

## Self-Hosting Status

The documentation hub describes Streamplace as open source and provides a
self-hosting track, but the linked installation URL currently redirects to the
hosted Streamplace application. No production deployment topology, storage
contract, capacity result, upgrade procedure, or moderation boundary was
verified from that guide.

Use hosted Streamplace for an integration rehearsal. Do not claim a
community-hosted production broadcast until a pinned release is deployed and
load-tested.

## Selection Gates

- Decide which plenary or rooms, if any, are public.
- Run a consented MiroTalk-to-compositor-to-Streamplace rehearsal over both
  RTMPS and WHIP; select one ingest path and document failover.
- Verify video composition, mixed audio, screen sharing, captions, reconnects,
  and content-warning behavior.
- Confirm the public playback URL and announcement can be projected safely into
  CA, My Community, calendars, and the brain without exposing ingest secrets.
- Load-test the selected hosted or self-hosted deployment at the event's actual
  broadcaster and viewer targets.
- Assign an operator and a backup operator with a tested stop-stream control.
- Decide whether involving the Streamplace team is a technical dependency,
  partnership, invitation, or none of these.

## Sources

- [Streamplace documentation](https://stream.place/docs/)
- [Streamplace quick start](https://stream.place/docs/guides/start-streaming/quick-start/)
- [Streamplace OBS ingest](https://stream.place/docs/guides/start-streaming/obs/)
- [Streamplace developer hub](https://stream.place/docs/developers/)
- [Streamplace multistreaming](https://stream.place/docs/features/multistreaming/)
- [MiroTalk RTMP documentation](https://docs.mirotalk.com/mirotalk-sfu/rtmp/)
