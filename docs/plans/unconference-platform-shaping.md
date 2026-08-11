---
shaping: true
---

# Unconference Platform - Shaping

**Status:** Selected pilot shape with production hosting decisions outstanding
**Date:** 2026-08-11

## Context

The Social Internet Unconference needs a shared workflow for forming an agenda,
finding times, running sessions, preserving what happened, and carrying useful
resources and reflections back into the community. No single application owns
the unconference. Harmonica, community-admin, Avails, My Community, Bluesky,
Jitsi, the selected transcription provider, Google Calendar, scenius-digest,
and GitHub each have a bounded role.

This shape is additive to the event planning in the repository README. It does
not settle the event name, date, scale, or whether the JaaS scheduled-session
pilot eventually changes the proposed MiroTalk main-event room stack.

## Requirements

| ID | Requirement | Status |
|---|---|---|
| R0 | Community members can use AI-facilitated conversations to propose unconference topics and sessions before the event. | Core goal |
| R1 | Similar proposals resolve to one stable topic card without losing contributing conversations or merge lineage. | Must-have |
| R2 | Each approved topic has one live surface at `/u/[slug]` and a Bluesky post; verified members can support it from either surface and linked identities count once. | Must-have |
| R3 | A topic becomes ready when it has at least three unique member supporters and at least one candidate time works for three of those supporters; an organizer must still promote it. | Must-have |
| R4 | Promotion produces a scheduled session with a Jitsi link, calendar invitations, an entry on the shared Google Calendar, and an event in My Community. | Must-have |
| R5 | Invited and authenticated attendees can access a private transcript after the session; recording and transcript processing are community infrastructure, not Harmonica infrastructure. | Must-have |
| R6 | The private transcript can inform authorized tools, while an approved public-safe synthesis and resources persist in `unconference-brain/`. | Must-have |
| R7 | Participants can run follow-up Harmonica conversations about one session or the unconference as a whole, with the relevant community sources as context. | Must-have |
| R8 | Public links found verbatim in transcripts can enter the My Community digest after review without exposing private quotes, identities, or transcript content. | Must-have |

## Decisions

| Decision | Resolution |
|---|---|
| Project type | Add immutable Harmonica project type `unconference` with public route `/u/[slug]`. |
| Community binding | Reuse the project's community-admin binding and fail-closed membership gate. |
| Topic ownership | `unconference-brain/` is canonical. Harmonica may generate/review candidates and hold an operational projection; CA owns the cross-system coordination twin. |
| Publication review | Exact duplicates may merge automatically. New cards and semantic merge suggestions require organizer review before publication. |
| Late merge | Keep the oldest canonical card. Preserve all Bluesky posts as vote sources, deduplicate likers, and redirect retired posts to the canonical card. |
| Harmonica vote identity | Count a verified Harmonica identity even without Bluesky. CA resolves email and DID votes through linked accounts at tally time. |
| Availability | Reusable community-scoped Avails. Topic support expresses willingness to attend; members do not repaint the same availability for every topic. |
| Scheduling threshold | Three unique member supporters and one candidate slot shared by three of those supporters. |
| Scheduling action | Passing the threshold marks a topic `ready`; only an organizer can promote it. |
| Conferencing | Pilot with JaaS for scheduled sessions. The scheduling request explicitly asks for it; Avails must not silently add Jitsi to unrelated bookings. This does not replace the proposed MiroTalk main-event stack. |
| Calendar | A community-hosted Google Calendar connector writes the CA event to the shared calendar idempotently. |
| Recording and transcription | The pilot uses temporary JaaS file recording, immediate SIU ingestion, and Deepgram Nova-3 behind a provider-neutral SIU adapter. Production evaluates self-hosted Voxtral and still requires community-hosted recording plus transcription, or an explicit R5 change. |
| Transcript access | Booked invitees have access immediately; authenticated room attendees are added after the session. |
| Transcript storage | Full transcripts remain private community sources. A private repository mirror is optional but cannot enforce per-session access. |
| Brain authority | `unconference-brain/` is a bidirectional canonical container. Valid human edits flow back through the GitHub App. |
| External side effects | Repository edits never publish a Bluesky post, schedule a call, send invitations, or expose a transcript without an explicit organizer action. |
| Harmonica role | Harmonica is one tool that consumes and contributes to community sources; it does not organize or own the unconference record. |

## Shape A: Community-Owned Coordination Loop

| Part | Mechanism | Flag |
|---|---|:---:|
| A1 | Add the Harmonica Unconference project type, `/u/[slug]`, community binding, and GitHub container adapter for `unconference-brain/`. | |
| A2 | Extract explicit proposals after linked Harmonica sessions, reconcile exact matches, put new cards or semantic merge suggestions through organizer review, and write accepted topic meaning and lineage into the brain. | |
| A3 | Create a CA coordination twin for each approved card. CA publishes the community Bluesky post and computes one member-only tally across Harmonica votes and Bluesky likes. | |
| A4 | Combine topic-specific support with reusable community availability. Show `ready` only when three unique supporters include a three-person overlap; require organizer promotion. | |
| A5 | Ask Avails to book the best slot and allocate a high-entropy JaaS room behind a CA join URL. CA issues room-scoped JWTs, consumes idempotent attendance/recording webhooks, materializes the event, and sends it to My Community and Google Calendar. | |
| A6 | Download the temporary JaaS recording into private SIU storage, transcribe it through a provider-neutral SIU adapter using Deepgram for the pilot, report artifact metadata idempotently to CA, and serve the normalized transcript under a per-session access policy. | |
| A7 | Persist an approved session synthesis and reviewed links in the brain; keep the full transcript private and available to authorized consumers such as Harmonica. | |
| A8 | Feed approved links through a new CA manual-links seam into scenius-digest and My Community; let follow-up Harmonica sessions consume individual-session or whole-project context. | |

## Fit Check: R x A

| Req | Requirement | Status | A |
|---|---|---|:---:|
| R0 | Community members can use AI-facilitated conversations to propose unconference topics and sessions before the event. | Core goal | ✅ |
| R1 | Similar proposals resolve to one stable topic card without losing contributing conversations or merge lineage. | Must-have | ✅ |
| R2 | Each approved topic has one live surface at `/u/[slug]` and a Bluesky post; verified members can support it from either surface and linked identities count once. | Must-have | ✅ |
| R3 | A topic becomes ready when it has at least three unique member supporters and at least one candidate time works for three of those supporters; an organizer must still promote it. | Must-have | ✅ |
| R4 | Promotion produces a scheduled session with a Jitsi link, calendar invitations, an entry on the shared Google Calendar, and an event in My Community. | Must-have | ✅ |
| R5 | Invited and authenticated attendees can access a private transcript after the session; recording and transcript processing are community infrastructure, not Harmonica infrastructure. | Must-have | ❌ |
| R6 | The private transcript can inform authorized tools, while an approved public-safe synthesis and resources persist in `unconference-brain/`. | Must-have | ✅ |
| R7 | Participants can run follow-up Harmonica conversations about one session or the unconference as a whole, with the relevant community sources as context. | Must-have | ✅ |
| R8 | Public links found verbatim in transcripts can enter the My Community digest after review without exposing private quotes, identities, or transcript content. | Must-have | ✅ |

**Unsolved:** R5 requires production decisions for both hosted services: replace
JaaS recording and Deepgram transcription with community-hosted mechanisms, or
explicitly relax those parts of the requirement. The provider-neutral adapter
and private-access mechanism are established.

## Lifecycle

```text
Harmonica proposal conversation
  -> topic candidate
  -> exact merge or organizer review
  -> canonical brain topic
  -> Harmonica /u projection
  -> CA coordination twin
  -> Bluesky post + /u card
  -> member support from either surface
  -> reusable Avails coverage
  -> ready (3 supporters + one 3-person overlap)
  -> organizer promotion
  -> Avails booking + Jitsi room
  -> CA event
      -> My Community event feed
      -> shared Google Calendar
      -> Deepgram transcription work
  -> private transcript
      -> participant access
      -> approved synthesis + links
      -> unconference-brain
      -> My Community digest links
      -> optional Harmonica reflection
```

## Brain Contract

```text
unconference-brain/
  README.md
  topics/
    <stable-topic-id>.md
  sessions/
    <stable-session-id>/
      README.md
      synthesis.md
      links.md
```

### Topic Files

Topic files hold canonical wording, lifecycle state, public Bluesky URL,
merge lineage, scheduled event reference, and public-safe provenance counts.
They do not hold voter identities or private conversation evidence.

Human-created files enter Harmonica as drafts and receive stable IDs. Draft
edits may sync directly. Changes to published or scheduled cards become pending
revisions. Deleting a file creates a review signal rather than deleting the
topic. The next mirror normalizes accepted human-named files to stable-ID paths.

### Session Files

Session files hold scheduled metadata, the related topic, an approved synthesis,
public results links, reviewed resources, and reflection history. They do not
hold attendee identities or private transcript text.

### Transcript Location

SIU storage holds private media and normalized transcripts. CA stores artifact
IDs, hashes, processing state, and access lists. CA grants short-lived
access after checking that the caller is a booked invitee or authenticated room
attendee. If a full transcript is mirrored into a private GitHub repository,
repository-wide readers will be able to see it; that is an explicit broadening
of access, not an implementation detail.

## Breadboard

### Places

| # | Place | Description |
|---|---|---|
| P1 | Unconference Project (Harmonica host) | Configure the project, connect the repository, launch proposal/reflection sessions, and review candidates. |
| P2 | Facilitated Conversation (Harmonica participant) | Propose or reflect through an AI-facilitated conversation. |
| P3 | `/u/[slug]` (public surface) | Browse topic cards, support topics, add reusable availability, and follow scheduled sessions. |
| P4 | Community Operations (CA organizer) | Configure integrations, review status, promote ready topics, and approve public links. |
| P5 | Standing Availability (Avails member) | Publish reusable community-scoped availability and booking trust. |
| P6 | Session Room (Jitsi) | Join and participate in a scheduled session with recording notice. |
| P7 | Session Record | Authorized transcript access and public-safe synthesis/resources. |
| P8 | My Community | See active proposal conversations, scheduled sessions, and digest resources. |
| P9 | GitHub Brain | Inspect and edit the durable community record. |
| P10 | Service Boundary | Harmonica, CA, Avails, connectors, scenius-digest, and their stores. |

### UI Affordances

| # | Place | Component | Affordance | Control | Wires Out | Returns To |
|---|---|---|---|---|---|---|
| U1 | P1 | project creation | Unconference project type | select | -> N1 | - |
| U2 | P1 | repository connection | Connect `social-internet-unconference` and choose `unconference-brain/` | click | -> N2 | - |
| U3 | P1 | session launcher | Start proposal conversation | click | -> N31 | - |
| U4 | P1 | topic review | Candidate card and source count | render | - | - |
| U5 | P1 | topic review | Approve, revise, merge, or reject | click | -> N5 | - |
| U6 | P2 | facilitated chat | Proposal/reflection conversation | respond | -> N3 | - |
| U7 | P3 | topic card | Canonical topic, support count, provenance count, and status | render | - | - |
| U8 | P3 | topic card | Support topic | click | -> N10 | - |
| U9 | P3 | topic card | Add or update availability | click | -> P5 | - |
| U10 | P3 | topic card | Ready/scheduled state and join link | render/click | -> P6 | - |
| U11 | P4 | integrations | Transcription provider and Google Calendar connector status | render/configure | -> N20, -> N21 | - |
| U12 | P4 | topic operations | Promote ready topic | click | -> N15 | - |
| U13 | P4 | event operations | Recording/transcription status and retry | render/click | -> N21 | - |
| U14 | P4 | link review | Extracted URL, metadata, and public-safe context | render | - | - |
| U15 | P4 | link review | Approve or reject digest link | click | -> N27 | - |
| U16 | P5 | standing availability | Weekly availability and trust setting | edit/save | -> N13 | - |
| U17 | P6 | room | Recording notice and consent state | render/accept | -> N19 | - |
| U18 | P6 | room | Join session | click | -> N19 | - |
| U19 | P7 | transcript | Private transcript | render | - | - |
| U20 | P7 | session record | Approved synthesis and resources | render | - | - |
| U21 | P7 | reflection | Reflect on this session | click | -> N31 | - |
| U22 | P8 | participation feed | Open Harmonica conversation and scheduled Jitsi event | render/click | -> P2, -> P6 | - |
| U23 | P8 | digest feed | Approved transcript-derived resource | render/click | - | - |
| U24 | P9 | GitHub | Brain files and edit controls | edit/commit | -> N8 | - |

### Code Affordances

| # | Place | Component | Affordance | Control | Wires Out | Returns To |
|---|---|---|---|---|---|---|
| N1 | P10 | Harmonica project types | Resolve `unconference` capabilities | call | -> S1 | -> U1 |
| N2 | P10 | Harmonica GitHub App | Create workspace/repository connection with unconference adapter | call | -> S2 | -> U2 |
| N3 | P10 | Harmonica session pipeline | Extract explicit topic proposals or reflection findings | call | -> N4 | - |
| N4 | P10 | topic reconciliation | Exact-match merge and semantic merge suggestion | call | -> S3 | -> U4 |
| N5 | P10 | topic review API | Apply organizer decision | call | -> S3, -> N6, -> N26 | -> U4 |
| N6 | P10 | CA topic sync | Upsert coordination twin by workspace/card external key | call | -> S4, -> N7 | - |
| N7 | P10 | CA ATProto service | Publish one Bluesky post or attach a late-merge post alias | call | -> S5 | -> U7 |
| N8 | P10 | GitHub webhook | Validate push, suppress App echo, and classify brain changes | receive | -> N9 | - |
| N9 | P10 | unconference container adapter | Parse valid topic/session changes into pending revisions | call | -> S3 | -> U4 |
| N10 | P10 | CA topic support API | Record verified Harmonica support identity | call | -> S6 | -> U7 |
| N11 | P10 | CA Bluesky tally | Fetch liker DIDs from every canonical/alias post | call | -> S6 | -> N12 |
| N12 | P10 | CA identity-aware tally | Resolve email/DID identities, filter to members, and count once | call | -> N14 | -> U7 |
| N13 | P10 | Avails standing availability | Save community-scoped availability record | call | -> S7, -> N14 | -> U16 |
| N14 | P10 | readiness evaluator | Require 3 unique supporters and one 3-person overlap | call | -> S4 | -> U10, -> U12 |
| N15 | P10 | CA promotion handler | Claim one organizer promotion and call Avails idempotently | call | -> N16 | - |
| N16 | P10 | Avails `schedule_call` | Select best slot for supported DIDs and request Jitsi | call | -> N17, -> S8 | -> N15 |
| N17 | P10 | Avails conferencing | Allocate a high-entropy JaaS room and include the stable CA join URL in ICS/result | call | -> S8 | -> N18 |
| N18 | P10 | CA event materializer | Create linked event exactly once from booking result | call | -> S9, -> N20, -> N21 | -> U10, -> U22 |
| N19 | P10 | CA JaaS gateway | Check identity/consent, issue room-scoped JWT, and consume authenticated idempotent attendance events | call/event | -> S10 | -> U17, -> U18 |
| N20 | P10 | CA connector registry | Configure/invoke Google Calendar connector with CA event idempotency key | call | -> S11 | -> U11 |
| N21 | P10 | CA connector registry | Configure the transcription integration, lease private-media work, and track retries | call | -> N22, -> S12 | -> U11, -> U13 |
| N22 | P10 | SIU transcription adapter | Submit private JaaS media to Deepgram, normalize diarized results, upload artifacts, and report metadata | call | -> N23, -> S13 | - |
| N23 | P10 | CA transcript callback | Verify connector, deduplicate callback, and set artifact ACL | receive | -> S12, -> S13, -> N24 | -> U13 |
| N24 | P10 | transcript processor | Produce draft synthesis and extract literal transcript URLs | call | -> S14, -> S15 | -> U14 |
| N25 | P10 | transcript access gate | Authorize booked invitee or authenticated attendee and issue short-lived URL | call | -> S13 | -> U19 |
| N26 | P10 | brain mirror | Write approved topic/session/synthesis/link files in one commit or none | call | -> S2 | -> U20, -> U24 |
| N27 | P10 | CA link review | Approve public-safe resource | call | -> S15, -> N26, -> N28 | -> U14 |
| N28 | P10 | CA manual-links endpoint | Return approved community links to trusted consumer | call | - | -> N29 |
| N29 | P10 | scenius-digest link merge | Merge CA links with Telegram/Slack and apply community visibility | call | -> S16 | -> U23 |
| N30 | P10 | Harmonica source import | Read authorized transcript or brain synthesis as project context | call | -> S17 | -> N3 |
| N31 | P10 | reflection launcher | Create session anchored to one session or whole project | call | -> S1 | -> P2 |

### Data Stores

| # | Place | Store | Description |
|---|---|---|---|
| S1 | P10 | Harmonica workspace/session records | Project type, linked sessions, community binding, and reflection anchors. |
| S2 | P10 | GitHub repository connection and brain tree | Installation, repository, managed path, commit SHAs, and files. |
| S3 | P10 | Unconference topic projection | Operational mirror of canonical wording, revisions, evidence references, and merge lineage held in `unconference-brain/`. |
| S4 | P10 | CA topic twins | Community, external card key, state, scheduling configuration, and event link. |
| S5 | P10 | Bluesky records | Canonical and alias post URIs/CIDs. |
| S6 | P10 | CA support observations | Harmonica identity votes and Bluesky liker DIDs, resolved at tally time. |
| S7 | P10 | Avails standing availability | Community-scoped member availability and trust records in member PDSes. |
| S8 | P10 | Avails booking ledger | Idempotent slot, participants, JaaS room name, stable CA join URL, and booking result. |
| S9 | P10 | CA events | Scheduled session projected to scenius-digest/My Community. |
| S10 | P10 | CA session participation ACL | Booked identities and authenticated room attendees. |
| S11 | P10 | Google Calendar event | Shared calendar event id and update/cancel state. |
| S12 | P10 | CA integration jobs | Connector, event, lease, status, retry, and result metadata. |
| S13 | P10 | Private transcript store | Media/transcript artifact, hash, and short-lived access service. |
| S14 | P10 | Draft session synthesis | Reviewable public-safe derivative of the transcript. |
| S15 | P10 | CA digest-link candidates | Verbatim URL, metadata, provenance, review state, and public-safe context. |
| S16 | P10 | scenius-digest links | Unified link feed consumed by My Community. |
| S17 | P10 | Harmonica project sources | Authorized transcript/synthesis context used by follow-up sessions. |

## Implementation Slices

The detailed slices, acceptance criteria, and repository boundaries are in
[Unconference Platform - Implementation Slices](./unconference-platform-slices.md).
The production recording/transcription hold is isolated to V6; the private
pilot can proceed.

| # | Slice | Demo |
|---|---|---|
| V1 | Unconference project and brain | Create an Unconference project, connect the SIU repo, and see a manually created draft topic round-trip through `unconference-brain/`. |
| V2 | Facilitated topic formation | Complete a proposal conversation, review a candidate, merge/approve it, and see the canonical card on `/u/[slug]`. |
| V3 | Bluesky and cross-surface support | Approve a card, publish its CIBC test post, support it in Harmonica and Bluesky, and see one member-aware tally. |
| V4 | Availability readiness | Add reusable availability and see the card move from `needs support` to `needs availability` to `ready` only at 3 supporters plus one 3-person overlap. |
| V5 | Promotion and distribution | Promote a ready topic and see its chosen time, Jitsi link, CA/My Community event, and shared Google Calendar entry. |
| V6 | Private session record | Run a recorded test session and let booked/attending identities open the private transcript while another member is denied. |
| V7 | Brain and digest return | Approve the synthesis and extracted links, see files in the brain, and see approved resources in My Community's digest. |
| V8 | Reflection loop | Launch an individual-session and whole-unconference reflection with the correct sources, then see approved findings return to the brain. |

## Spikes

- Complete: [Jitsi scheduling, identity, attendance, and recording](./spike-jitsi-scheduling-attendance.md)
- Rejected: [Meetily server fit](./spike-meetily-rejected.md)
- Complete: [Transcription provider decision](./spike-transcription-provider.md)

## Existing Seams To Reuse

- Harmonica immutable project types and capability registry.
- Harmonica workspace community binding and community-admin membership oracle.
- Harmonica GitHub App connection, repository ingestion, mirror commits, webhook
  echo suppression, and human-edit ingestion.
- Harmonica structured knowledge extraction and memory-style reconciliation.
- CA linked account/identity model and member DID resolution.
- CA per-community ATProto service accounts and Bluesky liker ingestion.
- CA event storage and `/api/events/manual` projection into scenius-digest.
- CA integrations/connectors design (`cai_` integration-bound tokens); the
  registry is designed but not yet built.
- Avails community-scoped standing availability, voter-DID booking,
  idempotency ledger, ICS generation, and interactive Google Calendar support.
- scenius-digest private-community filtering and My Community links/events APIs.

## Explicit Non-Goals For V1

- Automatically schedule as soon as thresholds pass.
- Require Bluesky authentication in Harmonica.
- Publish raw transcripts or private transcript quotes.
- Infer URLs that are not literal transcript substrings.
- Let repository edits trigger external side effects.
- Replace the event's full synchronous lobby or settle MiroTalk versus Jitsi
  without the room-stack decision.
- Build a generic integration marketplace before the transcription and Google
  connectors establish the contract.
