# SIU AV2 Event-Grant Consumer Contract

**Status:** Implemented 2026-08-20 in Community Admin PR #142 and Avails PR #179
**Date:** 2026-08-20
**Event DID:** `did:plc:mzvqnxye3oejamuwmfl4qvou`
**Producer:** Community Admin
**Consumer:** Avails

## Outcome

A participant who has an active SIU `event-participant` grant can publish one
standing-availability record for SIU. Community Admin remains the sole authority
for that grant. Avails consumes Community Admin's decision and never queries the
Bluesky relationship graph to infer, confirm, or refresh SIU participation.

The grant authorizes only these bridge capabilities:

- `proposal-support`, consumed inside Community Admin; and
- `publish-standing-availability`, consumed by Avails.

It does not grant Community Admin membership, organizer authority, room entry,
an invitation, attendance, transcript access, or permission to schedule a call.

## Stable Identifiers

- The authorization scope is the SIU DID, not Community Admin's local `siu`
  community ID and not the mutable `unconference.bsky.social` handle.
- The subject is the participant DID established by Avails' ATProto OAuth.
- Avails records use `{ "type": "ca-event", "value":
  "did:plc:mzvqnxye3oejamuwmfl4qvou" }`.
- `ca-event` is separate from `ca-community`; neither scope satisfies the other.
- Avails' deterministic scope key preserves one record per participant per event
  DID, so availability is entered once for SIU rather than once per topic.

## Producer Introspection

Community Admin exposes a bearer-gated online decision endpoint:

```http
POST /internal/event-participant-grants/introspect
Authorization: Bearer <CA_CONFIG_SECRET>
Content-Type: application/json

{
  "event_did": "did:plc:mzvqnxye3oejamuwmfl4qvou",
  "subject_did": "did:plc:participant",
  "capability": "publish-standing-availability"
}
```

A successful decision is `200` for both active and inactive grants:

```json
{
  "relationship": "event-participant",
  "event_did": "did:plc:mzvqnxye3oejamuwmfl4qvou",
  "subject_did": "did:plc:participant",
  "capability": "publish-standing-availability",
  "active": true,
  "reason": "active",
  "checked_at": "2026-08-20T12:00:00.000Z",
  "expires_at": "2026-08-20T12:05:00.000Z"
}
```

Inactive reasons are `not-following`, `blocking`, `blocked-by`, and `excluded`.
An unknown or unresolved actor is inactive and reported as `not-following`; the
contract does not disclose whether Community Admin had seen that DID before.

Other responses:

- `400`: malformed DID or unsupported capability.
- `401`: missing or invalid service credential.
- `404`: event DID is not bound to a Community Admin event.
- `503`: no unexpired observation exists and AppView could not refresh it.

Responses carry `Cache-Control: no-store`. They are authorization decisions for
the authenticated caller, not bearer grants or portable credentials, and cannot
be replayed as authority by another service.

## Verification And Expiry

Community Admin maps the event DID to exactly one local event binding, then reads
the existing event-participant row. The source observation TTL is controlled by
one shared `EVENT_GRANT_TTL_MS`, defaulting to five minutes. Proposal support and
Avails introspection must call the same decision function and use this same TTL.

When the observation is absent or expired, Community Admin calls the public
AppView `app.bsky.graph.getRelationships` with the SIU account as `actor` and the
participant DID in `others`. `followedBy` means the participant follows SIU;
`blocking` and `blockedBy` deny the grant. This is the orientation used by the
shipped AV1 producer.

Decision order is:

1. A durable organizer exclusion denies immediately without a graph refresh.
2. An unexpired positive or negative observation may be used until `expires_at`.
3. An absent or expired observation must be refreshed before granting.
4. A timeout, 429, malformed response, or other AppView failure returns `503`
   once the prior observation has expired. It never creates or extends a right.
5. A successful negative refresh preserves historical support and availability
   evidence but makes it inactive.

Avails does not cache an active response across writes. Every event-scoped create
or replacement is introspected online, which makes organizer exclusion effective
on the next write even when the underlying follow observation remains fresh.

## Avails Write Behavior

For `POST /api/availability` with a `ca-event` scope, Avails:

1. uses its authenticated `req.userDid` as `subject_did`;
2. validates that `scope.value` is a DID;
3. requests `publish-standing-availability` introspection from Community Admin;
4. writes to the participant's own PDS only when `active` is true; and
5. returns a non-authorizing error without writing in every other case.

The HTTP behavior is:

- active grant: existing `200`/`201` upsert behavior;
- inactive grant: `403` with the inactive reason;
- producer unavailable or expired verification: `503` with a retryable message;
- consumer misconfiguration: `503`, never a bypass.

`GET /api/availability/mine` and `DELETE /api/availability/:rkey` remain available
regardless of grant state. A participant must always be able to inspect or delete
their own record. Editing or extending an event record is a new write and requires
an active grant.

## Revocation And Restoration

- Unfollow or either-direction block becomes effective after explicit refresh or
  no later than the five-minute source TTL.
- Organizer exclusion is effective immediately in Community Admin and on the next
  Avails introspection; re-follow never clears it.
- Losing a grant does not delete or mutate the participant-owned Avails record.
- An inactive or expired record is excluded from readiness.
- If the grant later becomes active again, the existing unexpired record qualifies
  again without re-entry. An expired availability record still requires the
  participant to republish it.

## Readiness Contract

AV2 must not reuse `schedule_call`, because that operation can create calendar
artifacts and send invitations. Avails adds a read-only, service-authenticated
`evaluate_availability_overlap` operation with this input:

```json
{
  "scope": {
    "type": "ca-event",
    "value": "did:plc:mzvqnxye3oejamuwmfl4qvou"
  },
  "eligibleDids": ["did:plc:a", "did:plc:b", "did:plc:c"],
  "window": { "start": "2026-09-01", "end": "2026-09-30" },
  "durationMinutes": 60,
  "threshold": 3
}
```

Community Admin supplies only current proposal liker DIDs whose event grant is
active in the same evaluation pass. Avails reads only those participants' PDS
records matching the exact `ca-event` scope and returns aggregate overlap:

Until SIU has a fixed event date, the approved readiness window is the next 56
calendar days and the candidate session duration is 60 minutes. This rolling
window matches Avails' default standing-availability lifetime.

```json
{
  "ready": true,
  "threshold": 3,
  "eligibleSupporters": 3,
  "supportersWithRecords": 3,
  "maxOverlap": 3,
  "candidateSlot": "2026-09-10T16:00"
}
```

The operation has no booking ledger, ICS generation, email, invitation, poll, or
calendar side effect. `ready` is true only when at least three active supporters
share one slot. Three supporters without one shared three-person slot are not
ready. Community Admin may publish aggregate readiness and a generic reason, but
never DIDs, individual windows, or record contents on the public agenda.

## No-Bluesky-Query Invariant

For `ca-event` authorization and readiness, Avails must not call
`app.bsky.graph.getRelationships`, `getFollowers`, `getList`, or any equivalent
Bluesky social-graph endpoint. It may resolve participant DIDs to their PDS and
read their participant-owned availability records. Community Admin is the only
component that interprets a Bluesky follow or block as an SIU grant.

Tests must fail if an event path reaches Avails' Bluesky-list resolver or if a
Community Admin outage falls through to a graph lookup or permissive write.

## Approved Privacy Consequence

Avails currently stores standing availability in the participant's PDS, where the
record is public to anyone who already knows the DID and collection. AV2 keeps
that participant-owned storage model and adds clear UI disclosure; the SIU agenda
exposes only aggregate readiness. This satisfies the AV2 slice's public-agenda
boundary, but it does not make individual availability globally private.

SIU approved this bridge with explicit disclosure on 2026-08-20. If SIU later
requires the availability record itself to be private, that needs a larger storage
and revocation shape. Implementation must not imply that PDS records are private.

## Approved Review Gates

Approved together on 2026-08-20:

- `ca-event` plus the event DID is the correct stable scope;
- five minutes is the accepted maximum unfollow/block detection delay;
- online introspection with fail-closed `503` behavior is acceptable;
- organizer exclusion is immediate without deleting participant records;
- readiness is a read-only three-person overlap operation; and
- participant-owned public PDS storage with explicit disclosure is acceptable,
  or a separate private-storage shape is required.
