---
shaping: true
---

# SIU Atmospheric Groups Pilot - Shaping

**Status:** Shape D selected, breadboarded, and sliced
**Date:** 2026-08-12
**Tracks:** SIU #17, HAR-1546, community-admin #56

## Context

SIU already uses Harmonica as an open topic-proposal surface and plans to use
Community Admin, Bluesky, and Avails for identity-aware coordination. Community
Admin is moving toward Atmospheric Groups: DID-bearing groups with a swappable
Concierge and portable membership.

SIU is a temporary, cross-community event rather than an ordinary standing
community. The pilot must give the event portable identity and authority
without treating everyone who proposes, follows, supports, attends, or reads a
transcript as a permanent community member.

## Requirements

| ID | Requirement | Status |
|---|---|---|
| R0 | Give SIU a portable identity and public record that can outlive any one application or Concierge. | Core goal |
| R1 | Anyone can participate in the Harmonica topic-proposal session without first joining a community or using Bluesky. | Must-have |
| R2 | The Harmonica closing flow points to the SIU Bluesky profile; a verified follower can support proposals and add community-scoped standing availability. | Must-have |
| R3 | Following creates an event-participant relationship, not standing community membership, organizer authority, room admission, or transcript access. | Must-have |
| R4 | Organizers govern the SIU identity and grant Harmonica, Community Admin, and other applications only scoped, revocable authority. | Must-have |
| R5 | `unconference-brain/` remains canonical for topic and session meaning; protocol records are public-safe references and discovery artifacts. | Must-have |
| R6 | Public records contain no private responses, voter identities, availability details, invitations, attendee lists, or transcripts. | Must-have |
| R7 | The pilot does not block agenda distribution or require an unsettled lexicon in the live launch. | Must-have |
| R8 | New rights fail closed when follow verification or authorization is unavailable, while existing public agenda access remains available. | Must-have |

## Shapes

### A: SIU as a standing community

| Part | Mechanism | Flag |
|---|---|:---:|
| A1 | Provision SIU as a DID-bearing Atmospheric Group. | |
| A2 | Treat accepted participants or followers as ordinary SIU members. | |
| A3 | Use group membership for support, availability, room, and record access. | |

### B: Event owned by CIBC

| Part | Mechanism | Flag |
|---|---|:---:|
| B1 | Publish SIU records under the CIBC community DID. | |
| B2 | Use CIBC membership and roles as the event authorization boundary. | |
| B3 | Represent non-CIBC attendees as invitations outside the group model. | |

### C: Multi-group federation

| Part | Mechanism | Flag |
|---|---|:---:|
| C1 | Bind the event to multiple sponsoring community DIDs with no SIU DID. | |
| C2 | Accept qualifying membership from any sponsor for event actions. | |
| C3 | Publish references through each sponsoring community. | |

### D: Event DID with scoped relationships

| Part | Mechanism | Flag |
|---|---|:---:|
| D1 | Use the organizer-controlled SIU Bluesky account DID as the event's stable identity anchor. Its DID and PDS remain portable independently of Community Admin. | |
| D2 | Bind Community Admin to the SIU DID as the bridge coordinator. CA holds the SIU account app password under its existing per-community service-account pattern and exposes a narrow publish endpoint; no other service receives it. Scoped, revocable delegation replaces this in AV3. | |
| D3 | Community Admin point-checks the DID against the SIU DID with unauthenticated `app.bsky.graph.getRelationships`, then maintains a short-lived `event-participant` grant. Unfollow makes existing support/availability stop qualifying; block or organizer exclusion overrides follow, and exclusion survives re-follow. | |
| D4 | Give `event-participant` exactly two initial capabilities: support SIU proposals and publish standing availability for SIU scheduling. | |
| D5 | Keep proposal participation open in Harmonica; its closing turn links to the SIU Bluesky profile as the optional next step for support and availability. | |
| D6 | On steward approval, Harmonica calls CA's publish endpoint; CA posts from the SIU account and returns the AT URI/CID, written into the topic file before mirroring. Posts link to the canonical agenda/topic; lifecycle and results remain canonical in `unconference-brain/`, with outcome links posted as replies. Scheduled sessions may later use the shared calendar lexicon. | |
| D7 | Model invitation, room attendance, transcript access, and organizer authority as separate grants with their own provenance and revocation. | |
| D8 | Keep private bodies and relationship rosters in the appropriate private or permissioned plane; public records expose only aggregate or non-sensitive references. | |

## Fit Check

| Req | Requirement | Status | A | B | C | D |
|---|---|---|:---:|:---:|:---:|:---:|
| R0 | Give SIU a portable identity and public record that can outlive any one application or Concierge. | Core goal | ✅ | ❌ | ❌ | ✅ |
| R1 | Anyone can participate in the Harmonica topic-proposal session without first joining a community or using Bluesky. | Must-have | ❌ | ❌ | ✅ | ✅ |
| R2 | The Harmonica closing flow points to the SIU Bluesky profile; a verified follower can support proposals and add community-scoped standing availability. | Must-have | ✅ | ❌ | ❌ | ✅ |
| R3 | Following creates an event-participant relationship, not standing community membership, organizer authority, room admission, or transcript access. | Must-have | ❌ | ✅ | ❌ | ✅ |
| R4 | Organizers govern the SIU identity and grant Harmonica, Community Admin, and other applications only scoped, revocable authority. | Must-have | ✅ | ❌ | ❌ | ✅ |
| R5 | `unconference-brain/` remains canonical for topic and session meaning; protocol records are public-safe references and discovery artifacts. | Must-have | ✅ | ✅ | ❌ | ✅ |
| R6 | Public records contain no private responses, voter identities, availability details, invitations, attendee lists, or transcripts. | Must-have | ✅ | ✅ | ❌ | ✅ |
| R7 | The pilot does not block agenda distribution or require an unsettled lexicon in the live launch. | Must-have | ❌ | ❌ | ❌ | ✅ |
| R8 | New rights fail closed when follow verification or authorization is unavailable, while existing public agenda access remains available. | Must-have | ✅ | ✅ | ❌ | ✅ |

**Notes:**

- A fails R1 and R3 because it turns event participation into standing group membership.
- B fails R0 and R4 because CIBC, rather than SIU, owns the identity and authority.
- C has no stable event authority or single publication location and makes rights depend on unrelated sponsor memberships.
- D is selected and passes the fit check. The linked spike records the bridge contracts and remaining standards follow-ups before breadboarding.

## Selected Participant Journey

1. Anyone opens and completes the Harmonica proposal conversation.
2. Harmonica confirms the proposal recap and, at the end of the flow, shares the SIU Bluesky profile.
3. The participant follows the SIU profile if they want to support proposals or contribute standing availability.
4. Community Admin verifies the follower DID and grants an event-scoped participant relationship.
5. The participant supports proposals through Bluesky and publishes standing availability through Avails.
6. Invitation, room entry, attendance, and private transcript access are granted separately as the event progresses.

Community Admin rechecks the relationship on a bounded TTL. Unfollowing does
not delete support or availability evidence, but those records stop qualifying
for readiness. Either-direction blocks and organizer exclusion deny the grant;
an exclusion tombstone survives a later re-follow.

## Relationship Model

| Relationship | How acquired | Initial capabilities | Not implied |
|---|---|---|---|
| Organizer/steward | Explicit SIU governance grant | Configure the event, delegate applications, promote topics, revoke grants | Ownership by any application |
| Proposer | Completion of a Harmonica proposal | Contribute a candidate topic and approve its recap | Event participation credential or identity linkage to a DID |
| Event participant | Verified follow of the SIU Bluesky profile | Support proposals; publish SIU standing availability | Community membership, room entry, transcript access, organizer authority |
| Invitee | Explicit session/event invitation | Receive scheduling and room-access instructions | Attendance or transcript access after policy changes |
| Attendee | Authenticated room attendance | Attendance provenance; transcript eligibility under the session policy | Standing membership or public disclosure |
| Transcript reader | Booked invitee or authenticated attendee under the selected policy | Short-lived access to one private artifact | Access to other sessions or publication rights |

## Authority And Data Placement

| Data or action | Authority / home |
|---|---|
| Event identity and public profile | Organizer-controlled SIU Bluesky account DID and public repo |
| Bridge coordination | Community Admin binding to the SIU DID; CA holds the SIU app password under its per-community pattern, with no claimed `#concierge` registration until scoped delegation lands |
| Topic and session meaning | `unconference-brain/` |
| Public topic/session/results discovery | CA-published proposal posts (triggered by approval) holding canonical URLs; AT URI/CID written into the topic file; outcome replies link to scheduled sessions or results |
| Follow observation and event-participant grant | Community Admin point check through `app.bsky.graph.getRelationships` plus a TTL grant and durable organizer-exclusion tombstone until a stable portable contract exists |
| Proposal support tally | Community Admin coordination state; publish aggregates only |
| Standing availability | Participant-controlled Avails records; expose overlap/results, not schedules |
| Invitations, attendance, transcript ACLs | Private Community Admin / event infrastructure state |
| Private responses and transcripts | Harmonica and SIU private stores under their existing access policies |

## Next

Plan and implement AV1 from `atmospheric-groups-pilot-slices.md` as the identity
portion of SIU platform V3. It is a bridge rehearsal: no new lexicon and no
DID-document mutation; the only service credential is CA's existing
per-community app-password pattern, held solely by CA and replaced by scoped
delegation in AV3.

## Breadboard

### Places

| # | Place | Description |
|---|---|---|
| P1 | Harmonica proposal conversation | Open participant flow for shaping and confirming a topic proposal. |
| P2 | SIU public agenda | Canonical public topic cards and proposal status. |
| P3 | SIU Bluesky profile and proposal post | Optional follow and support surface after proposal formation. |
| P4 | Avails standing availability | Participant-controlled SIU availability. |
| P5 | Community Admin operations | Organizer registration, exclusions, tally/readiness, and event coordination. |
| P6 | Service boundary | Harmonica, Community Admin, Bluesky AppView, Avails, and GitHub brain stores. |

### UI Affordances

| # | Place | Component | Affordance | Control | Wires Out | Returns To |
|---|---|---|---|---|---|---|
| U1 | P1 | Harmonica chat | Open proposal conversation | navigate | -> P1 | - |
| U2 | P1 | Harmonica chat | Proposal exchange and confirmed recap | respond | -> N1 | - |
| U3 | P1 | Harmonica closing turn | SIU Bluesky profile link and explanation of follow benefits | click | -> P3 | - |
| U4 | P2 | Unconference topic card | Canonical topic, state, support count, and Bluesky link | render/click | -> P3 | - |
| U5 | P3 | Bluesky profile | Follow SIU | click | -> N5 | - |
| U6 | P3 | Bluesky proposal post | Like proposal | click | -> N6 | - |
| U7 | P4 | Avails | SIU standing availability editor | save | -> N9 | - |
| U8 | P4 | Avails | Active/inactive SIU participation status | render | - | - |
| U9 | P5 | CA topic operations | Publish result: SIU post link and state per topic | render | - | - |
| U10 | P5 | CA participant operations | Exclude or restore participant DID | click | -> N8 | - |
| U11 | P5 | CA topic operations | Qualifying support count and readiness reason | render | - | - |

### Code Affordances

| # | Place | Component | Affordance | Control | Wires Out | Returns To |
|---|---|---|---|---|---|---|
| N1 | P6 | Harmonica proposal pipeline | Extract/reconcile confirmed proposal | call | -> S1 | -> U2 |
| N2 | P6 | Harmonica public record | Render canonical topic projection | read | -> S1 | -> U4 |
| N3 | P6 | CA publish endpoint | Post from the SIU account on approval (idempotent per topic), return and bind AT URI/CID | call | -> S2, -> S3 | -> U9, U4 |
| N4 | P6 | CA tally refresh | Fetch unique liker DIDs across canonical and alias posts | call | -> N6 | -> N10 |
| N5 | P6 | Bluesky graph | Follow record under participant DID | write | -> S3 | -> N7 |
| N6 | P6 | Bluesky AppView | `getLikes` returns liker DIDs | read | -> S3 | -> N4 |
| N7 | P6 | CA follow verifier | `getRelationships(participantDid, [siuDid])` plus block state | call | -> S4 | -> N10, N9, U8 |
| N8 | P6 | CA participant policy | Write/clear durable organizer-exclusion tombstone | call | -> S4 | -> N10, N9 |
| N9 | P6 | Avails authorization | Require active event-participant grant before accepting SIU availability | call | -> S5 | -> U7, U8 |
| N10 | P6 | CA readiness evaluator | Count active liker DIDs and evaluate overlap from Avails | call | -> S2 | -> U11, U4 |

### Data Stores

| # | Place | Store | Description |
|---|---|---|
| S1 | P6 | `unconference-brain/` plus Harmonica projection | Canonical topic meaning, lifecycle, evidence references, and merge lineage. |
| S2 | P6 | CA SIU coordination state | Event DID binding, stable topic ID, AT URI/CID aliases, tally, readiness, and timestamps. |
| S3 | P6 | Bluesky public graph | SIU follow records, proposal posts, likes, and block records. |
| S4 | P6 | CA event-participant grants | Bounded-TTL follow/block decision plus durable organizer-exclusion tombstone. |
| S5 | P6 | Avails participant records | DID-controlled SIU standing availability; individual schedules remain private. |

### Wiring Check

- Open proposal path: U1 -> U2 -> N1 -> S1 -> N2 -> U4.
- Optional participant path: U3/U4 -> P3 -> U5 -> N5 -> S3 -> N7 -> S4 -> U8.
- Support path: U6 -> S3 -> N6 -> N4 -> N7 -> N10 -> S2 -> U11/U4.
- Availability path: U7 -> N9 -> N7/S4 -> S5; N10 reads overlap without exposing individual schedules.
- Exclusion path: U10 -> N8 -> S4 -> N10/N9; re-follow does not clear S4's tombstone.
