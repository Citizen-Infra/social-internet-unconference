---
shaping: true
---

# Unconference Platform - Implementation Slices

**Status:** Ready for issue breakdown; production recorder hold applies to V6
**Date:** 2026-08-11
**Shape:** [Unconference Platform](./unconference-platform-shaping.md)

## Slicing Rule

Each slice ends in observable behavior across the smallest necessary set of
systems. Infrastructure that has no visible effect is part of the first slice
that exercises it, not a separate foundation project. Stable IDs and
idempotency keys are introduced when first needed and reused thereafter.

The JaaS path is approved for the pilot. It does not satisfy the production R5
requirement that recording infrastructure be community-hosted. V6 may ship to a
private pilot but cannot be called production-complete until that requirement is
resolved.

## Sequence

| Slice | Outcome | Primary repositories | Depends on |
|---|---|---|---|
| V1 | Unconference project and brain round-trip | Harmonica, SIU planning/brain repo | - |
| V2 | Facilitated topic formation and review | Harmonica | V1 |
| V3 | Bluesky publication and identity-aware support | Harmonica, community-admin | V2 |
| V4 | Reusable availability and readiness | community-admin, Avails, Harmonica | V3 |
| V5 | Organizer promotion and event distribution | community-admin, Avails, scenius-digest | V4 |
| V6 | Private recorded session and transcript access | community-admin, SIU connector/infra | V5 |
| V7 | Approved synthesis and digest return | community-admin, SIU brain, scenius-digest | V6 |
| V8 | Session and whole-project reflection loop | Harmonica, SIU brain | V7 |

## V1: Unconference Project And Brain

**Demo:** Create an Unconference project, bind its community and GitHub
repository, create a draft topic file, and see the same stable draft in
Harmonica and `unconference-brain/topics/` after edits from either side.

| Area | Scope |
|---|---|
| Harmonica UI | Add immutable `unconference` project type and `/u/[slug]` shell; expose community and repository setup. |
| Harmonica code | Add the managed-container adapter, topic schema/parser, stable ID allocation, App-commit echo suppression, and human-edit ingestion. |
| Brain | Create `unconference-brain/README.md`, `topics/`, and `sessions/` contracts without voter, attendee, or transcript bodies. |
| Affordances | U1, U2, U24; N1, N2, N8, N9, N26; S1-S3. |

Acceptance:

- A human-named draft gets one stable topic ID and normalizes to that path.
- Replaying a GitHub webhook or Harmonica mirror commit creates no duplicate.
- Invalid files produce a reviewable error and do not partially update state.
- Published-state edits become pending revisions; repository deletion never
  triggers an external side effect.

## V2: Facilitated Topic Formation

**Demo:** Complete one Harmonica proposal conversation, review the extracted
candidate, approve or merge it, and see one canonical topic card at `/u/[slug]`.

| Area | Scope |
|---|---|
| Conversation | Add proposal-session launcher and explicit topic extraction with source references. |
| Reconciliation | Auto-merge exact duplicates; present semantic suggestions and new cards for organizer review. |
| Review | Approve, revise, merge, or reject while preserving occurrences and merge lineage. |
| Affordances | U3-U7; N3-N6, N26; S1-S4. |

Acceptance:

- Exact duplicate submissions add occurrences without a second card.
- Semantic matches never merge without organizer action.
- The oldest canonical topic survives a merge and records all retired IDs.
- The brain and `/u` projection show the accepted wording and lineage.

## V3: Bluesky And Cross-Surface Support

**Demo:** Approve a topic, publish it through the community Bluesky account,
support it in Harmonica and Bluesky with linked identities, and see one unique
member tally.

| Area | Scope |
|---|---|
| CA foundation | Build the minimum topic-twin endpoint and community-bound service authentication exercised by this slice. |
| Publication | Publish one canonical post per approved topic; retain alias post URIs after late merges. |
| Support | Record verified Harmonica support, ingest liker DIDs, resolve linked accounts at tally time, and exclude non-members. |
| Affordances | U7, U8; N6, N7, N10-N12; S4-S6. |

Acceptance:

- Replaying topic sync or publication does not create another post.
- One linked person supporting on both surfaces counts once.
- A verified member without Bluesky can count through Harmonica.
- Unlinked or non-member Bluesky likes remain observations but do not count.
- A late merge aggregates every alias post without republishing.

## V4: Availability Readiness

**Demo:** Three supporters publish reusable community availability and the topic
moves through `needs support`, `needs availability`, and `ready` only when one
slot works for three supporters.

| Area | Scope |
|---|---|
| Avails | Expose reusable community-scoped standing availability and overlap evaluation for a supplied supporter set. |
| CA | Recompute readiness after support, identity-link, membership, or availability changes. |
| Harmonica | Show counts and readiness reason without exposing individual availability. |
| Affordances | U7-U9, U16; N12-N14; S4, S6, S7. |

Acceptance:

- Availability is entered once per community, not once per topic.
- Three supporters with no shared three-person slot are not ready.
- Removing support, membership, or overlap moves a non-scheduled topic back.
- Readiness never schedules or sends invitations automatically.

## V5: Promotion And Distribution

**Demo:** An organizer promotes a ready topic and receives one chosen time, CA
join URL, ICS invitation, CA/My Community event, and shared Google Calendar
event.

| Area | Scope |
|---|---|
| CA connector foundation | Implement the minimum integration registry, `cai_` auth, status/ping, and Google Calendar work path used here. |
| Avails | Extend `schedule_call` with explicit JaaS conference allocation, stable booking ID, random room name, CA join URL, and replay-safe result. |
| CA | Claim promotion once, materialize the event, configure JaaS, issue room-scoped JWTs after access/consent checks, and consume attendance webhooks. |
| Distribution | Project the CA event into My Community and the shared Google Calendar; handle update and cancellation. |
| Affordances | U10-U12, U17, U18, U22; N15-N20; S8-S11. |

Acceptance:

- Only an organizer can promote and only a ready topic can be promoted.
- Retrying promotion returns the same booking, room, event, and calendar entry.
- ICS and calendars contain the stable CA join URL, never a JWT.
- JaaS tokens use the CA account ID and literal room; only organizer tokens can
  moderate or record.
- Webhook duplicates and out-of-order delivery do not corrupt attendance.
- Cancellation stops token issuance and updates every event projection.

## V6: Private Session Record

**Demo:** Run a consented recorded pilot session, ingest the recording, lease it
to the SIU worker, and let invitees and authenticated attendees open the private
transcript while another member and a non-member are denied.

| Area | Scope |
|---|---|
| JaaS ingestion | Consume recording events, immediately persist and hash media in SIU storage, and protect the 24-hour source expiry. |
| Connector | Add kind-specific CA lease/progress/result routes and a single-job SIU worker using the pinned Meetily-derived import pipeline. |
| Artifacts | Implement encrypted media/transcript storage, lifecycle deletion, normalized `siu.transcript.v1`, and short-lived single-artifact reads. |
| Access | Build the CA transcript gate from booked identities and verified `PARTICIPANT_JOINED` observations. |
| Affordances | U11, U13, U17-U20; N19, N21-N25; S10, S12-S14. |

Acceptance:

- The same recording job cannot produce two transcript artifacts.
- Input and output hashes, engine version, upstream commit, and model are stored.
- Speaker fields remain null unless a verified diarization mechanism is added.
- CA stores references and ACLs, not media or transcript bodies.
- Media follows explicit deletion policy and temporary worker files are wiped.
- The authorization matrix in the demo passes.

**Production hold:** Replace JaaS recording with community-hosted recording or
explicitly relax R5 before marking this slice production-complete.

## V7: Brain And Digest Return

**Demo:** Review a draft synthesis and one literal transcript URL, approve them,
see the corresponding session files in `unconference-brain/`, and see the link
in My Community's digest without private transcript content.

| Area | Scope |
|---|---|
| Processing | Generate a private draft synthesis and extract only literal URLs with session/time provenance. |
| Review | Let organizers approve, revise, or reject synthesis and link candidates independently. |
| Brain | Atomically write `sessions/<id>/README.md`, `synthesis.md`, and `links.md`. |
| Digest | Add a trusted CA manual-links feed and merge approved links under existing community visibility rules. |
| Affordances | U14, U15, U20, U23, U24; N24, N26-N29; S14-S16. |

Acceptance:

- No transcript quote, attendee identity, or unapproved URL reaches the brain or
  digest.
- Every approved URL is a literal transcript substring.
- Replay creates no duplicate brain entry or digest link.
- A failed brain commit publishes neither partial files nor a digest result.
- Private-community visibility fails closed if CA config cannot be fetched.

## V8: Reflection Loop

**Demo:** Launch one reflection about a session and one about the whole
unconference, verify each receives only authorized sources, and approve a
finding back into the brain.

| Area | Scope |
|---|---|
| Harmonica | Add session/project reflection launchers and source selection by stable session ID. |
| Authorization | Import private transcripts only through the same CA gate; use public syntheses when private access is absent. |
| Knowledge return | Extract reflection findings through the existing review and brain mirror path. |
| Affordances | U3, U21, U22; N3-N5, N26, N30, N31; S1-S3, S17. |

Acceptance:

- A session reflection cannot read another session's private transcript.
- Whole-project context includes approved brain records and only caller-authorized
  private sources.
- Harmonica stores source references and derivatives, not an independent
  canonical transcript copy.
- Reflection output has no external side effect until organizer approval.

## Cross-Slice Invariants

- Stable topic, session, event, booking, connector-job, and artifact IDs are
  never derived from mutable titles or slugs.
- Every external side effect has an idempotency key and replay test.
- Community and membership checks fail closed.
- Raw support identities, availability, attendees, media, and transcripts never
  enter `unconference-brain/`.
- CA coordinates and authorizes; community connectors compute; SIU artifact
  storage holds private bodies; Harmonica consumes only authorized sources.
- Human review gates semantic merges, publication, scheduling, public synthesis,
  and digest links.
