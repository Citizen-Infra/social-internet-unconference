---
shaping: true
---

# SIU Atmospheric Groups Pilot - Implementation Slices

**Status:** Ready for implementation planning
**Date:** 2026-08-12
**Shape:** [SIU Atmospheric Groups Pilot](./atmospheric-groups-pilot-shaping.md), Shape D
**Spike:** [Atmospheric Event Identity](./spike-atmospheric-event-identity.md)

## Sequence

| Slice | Outcome | Primary repositories | Depends on |
|---|---|---|---|
| AV1 | Event DID and follower-qualified proposal support | community-admin, Harmonica | SIU platform V2 |
| AV2 | One event-participant grant authorizes Avails | community-admin, Avails, Harmonica | AV1 |
| AV3 | Portable publication and Concierge delegation | community-admin, Harmonica | Stable shared contracts; not required for SIU launch |

AV1 replaces the membership-specific identity portion of SIU platform V3. AV2
replaces the membership-specific authorization portion of V4. AV3 is the
Atmospheric Groups destination and must not be pulled into the launch bridge.

## AV1: Event DID And Follower-Qualified Support

**Demo:** An organizer configures the SIU Bluesky DID, then approves a topic;
Harmonica calls CA's publish endpoint and the post appears from the SIU account
with its AT URI/CID written into the topic file. A second DID follows SIU and
likes the post; the public topic count rises by one. The DID unfollows and the
count falls while its historical like remains. Re-follow restores it unless an
organizer exclusion or either-direction block applies.

### UI Affordances

| # | Place | Affordance | Control | Wires Out | Returns To |
|---|---|---|---|---|---|
| U4 | P2 | Canonical topic, qualifying support count, and Bluesky link | render/click | -> P3 | - |
| U5 | P3 | Follow SIU | click | -> N5 | - |
| U6 | P3 | Like proposal | click | -> N6 | - |
| U9 | P5 | Publish result: SIU post link and state per topic | render | - | - |
| U10 | P5 | Exclude or restore participant DID | click | -> N8 | - |
| U11 | P5 | Qualifying support count and reason | render | - | - |

### Code Affordances

| # | Place | Affordance | Control | Wires Out | Returns To |
|---|---|---|---|---|---|
| N2 | P6 | Render canonical topic projection | read | -> S1 | -> U4 |
| N3 | P6 | Post from the SIU account on approval (idempotent per topic), return and bind AT URI/CID | call | -> S2, -> S3 | -> U9, U4 |
| N4 | P6 | Fetch unique liker DIDs across canonical and alias posts | call | -> N6 | -> N10 |
| N5 | P6 | Bluesky follow record | write | -> S3 | -> N7 |
| N6 | P6 | `getLikes` returns liker DIDs | read | -> S3 | -> N4 |
| N7 | P6 | `getRelationships(participantDid, [siuDid])` plus block state | call | -> S4 | -> N10 |
| N8 | P6 | Write/clear durable organizer-exclusion tombstone | call | -> S4 | -> N10 |
| N10 | P6 | Count unique active event-participant liker DIDs | call | -> S2 | -> U11, U4 |

### Stores

| # | Place | Store | Scope |
|---|---|---|---|
| S1 | P6 | Canonical brain plus Harmonica projection | Existing; read only in this slice |
| S2 | P6 | CA SIU coordination state | Event DID, topic ID, AT URI/CID aliases, tally, timestamps |
| S3 | P6 | Bluesky public graph | Existing external follows, likes, posts, and blocks |
| S4 | P6 | CA event-participant grants | TTL relationship decision and exclusion tombstone |

Acceptance:

- The SIU DID is stored separately from Community Admin's local community ID.
- Repeated approval/publish calls for one topic return the same post URI; no duplicate posts or associations are created.
- Only CA holds the SIU account app password (its per-community service-account pattern); Harmonica receives only the returned AT URI/CID and never sees the credential.
- Only a liker DID with a current follow and no applicable block or exclusion counts.
- Unfollow or block preserves the observation but removes it from the qualifying count after the bounded TTL or explicit refresh.
- Organizer exclusion overrides follow and survives re-follow until explicitly cleared.
- AppView timeout or 429 never grants a new right and never deletes historical observations.
- Public UI exposes aggregate counts and reasons, never follower/liker rosters or exclusions.

## AV2: Shared Event Grant Authorizes Avails

**Demo:** A qualifying SIU follower opens Avails and saves SIU standing
availability. A non-follower is denied. Three active supporters with a shared
slot move a topic to ready; unfollowing makes that support and availability stop
qualifying and moves an unscheduled topic back.

### UI Affordances

| # | Place | Affordance | Control | Wires Out | Returns To |
|---|---|---|---|---|---|
| U4 | P2 | Topic readiness state and reason | render | - | - |
| U7 | P4 | SIU standing availability editor | save | -> N9 | - |
| U8 | P4 | Active/inactive SIU participation status | render | - | - |
| U11 | P5 | Qualifying support count and readiness reason | render | - | - |

### Code Affordances

| # | Place | Affordance | Control | Wires Out | Returns To |
|---|---|---|---|---|---|
| N7 | P6 | Read/revalidate CA event-participant grant | call | -> S4 | -> N9, N10, U8 |
| N9 | P6 | Require active grant before accepting SIU availability | call | -> S5 | -> U7, U8 |
| N10 | P6 | Evaluate active supporter overlap from Avails | call | -> S2 | -> U11, U4 |

### Stores

| # | Place | Store | Scope |
|---|---|---|---|
| S2 | P6 | CA SIU coordination state | Existing from AV1; adds readiness result |
| S4 | P6 | CA event-participant grants | Existing from AV1; shared authorization fact |
| S5 | P6 | Avails participant records | DID-controlled SIU standing availability |

Acceptance:

- Avails and proposal tally consume the same Community Admin grant semantics; neither performs an independent Bluesky follow check.
- Availability is scoped to the SIU DID and entered once, not once per topic.
- A lost grant stops availability from qualifying but does not delete the participant's record.
- Three active supporters without a shared three-person slot are not ready.
- Readiness changes never schedule or invite automatically.
- Individual availability and participant identity are not exposed on the public agenda.

## AV3: Portable Publication And Concierge Delegation

**Demo:** After shared contracts stabilize, replace the bridge writer with a
scoped, revocable request through the SIU DID's discoverable Concierge and let a
second compatible application resolve the same event/session reference without
calling a Harmonica-specific or Community Admin-specific endpoint.

This slice is deliberately deferred. It requires evidence from Community Admin
#57/#59/#60 and HAR-1423. It must preserve AV1/AV2 stable topic IDs, canonical
URLs, and event-participant semantics rather than migrating users to a new
meaning of membership.

Acceptance:

- The SIU DID identifies a discoverable, replaceable Concierge through a stable shared contract.
- Harmonica or another app submits a signed, capability-scoped request; no whole-repo app password is shared.
- A shared public-safe event/session reference can be consumed by another application.
- Replacing Community Admin does not change the SIU DID, canonical topic IDs, or public record URIs.
- Private participant data remains outside public records.
