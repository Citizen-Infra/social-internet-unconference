# Process Notes — social-internet-unconference

## 2026-07-22 — Planning repo created from the Erlend call
- **Done:** Created the private planning repo (README + 8 issues) from the 2026-07-21 Erlend Sogge Heggen call. README marks every unsettled item as unsettled; issues cover name, Streamplace contact, themes, the Roomy space, invite list, date/format, plus two added later — co-organisers from CIBC's partners (#7) and using Harmonica for agenda-forming and session reflection (#8).
- **Decisions:** Private now, public later — nothing is named or dated, and unconfirmed contacts are named freely. Deliberately **no invented facts**: no event name, no date, no headcount, one locked theme of an implied three or four. Erlend explicitly does not coordinate; Artem drives. Split #5 (invite list) from #7 (co-organisers) once they diverged.
- **State:** Clean and pushed. Nothing has moved externally — **#2 (contact Eli about Streamplace) is unassigned and gates format, date and scale.**
- **Next:** #2 is the first real action. Check in with Erlend on the Roomy space. Approach Newspeak House as the strongest co-organiser candidate.

## 2026-08-11 — Unconference platform shaped across the community stack
- **Done:** Added `docs/plans/unconference-platform-shaping.md`, a cross-system shape and breadboard for async topic formation, reviewed deduplication, the `/p/[slug]` Unconference record surface, Bluesky support, reusable Avails, organizer-promoted scheduling, CA/My Community events, community-hosted Meetily transcription, the bidirectional `unconference-brain/`, digest-link return, and follow-up reflections. Added standalone Jitsi and Meetily spike briefs.
- **Decisions:** No single tool owns the unconference. Harmonica owns facilitated topic meaning but is only one consumer/contributor; CA coordinates community identity, topic twins, events and connectors; Avails schedules; SIU infrastructure records/transcribes; the GitHub brain is the durable public-safe record. Readiness is three unique member supporters plus one candidate slot shared by three, followed by explicit organizer promotion. Full transcripts remain private and accessible to booked invitees plus authenticated attendees; approved syntheses and literal transcript links may become public.
- **State:** Shape A selected. R4 (Jitsi scheduling/attendance) and R5 (Meetily recording/transcript delivery) remain failed in the fit check until their spikes establish concrete mechanisms. The existing MiroTalk proposal is not silently displaced.
- **Next:** Run the Jitsi and Meetily spikes, update the shape and breadboard with their findings, then promote the provisional V1-V8 boundaries into a slices document.

## 2026-08-11 — JaaS pilot closes the Jitsi spike
- **Done:** Verified JaaS room-scoped JWTs, stable participant identity, idempotent join/leave and recording webhooks, hosted file recording, and the 24-hour recording download seam. Expanded the Jitsi spike into concrete Avails booking, CA join, attendance, consent, media ingestion, retry, update, and cancellation contracts.
- **Decisions:** Pilot scheduled sessions on 8x8 JaaS. Avails allocates the opaque room name and returns a stable CA join URL; CA owns JaaS keys, issues short-lived JWTs after membership and consent checks, and consumes webhooks. JWTs and direct recording URLs never enter ICS, calendars, logs, or the brain. This does not replace the proposed MiroTalk main-event stack.
- **State:** The Jitsi mechanism now satisfies R4 in the shape. JaaS temporarily processes and stores pilot media, so R5 remains open until the Meetily adapter proves private media ingestion through transcript delivery and production either moves recording onto community infrastructure or explicitly relaxes that requirement.
- **Next:** Run the Meetily connector spike from the private SIU media artifact produced by the JaaS ingestion worker.

## 2026-08-11 — Meetily spike selects an SIU-owned headless adapter
- **Done:** Verified the Meetily v0.4.0 release, license, supported Tauri architecture, beta audio-import pipeline, transcript output, and open webhook request against the upstream GitHub repository. Replaced the spike questions with deployable components, exact connector work/result contracts, transcript schema, artifact access and deletion rules, failure handling, and a production smoke.
- **Decisions:** Pin the MIT Community code at `0281737d87d26352fb0adc78c8c0975f691b23d1` and extract/refactor only its decoding, VAD, local transcription, and timestamped-segment pipeline into an SIU-owned single-job worker. Do not deploy the archived unauthenticated FastAPI/Docker backend, automate the desktop app, claim speaker attribution, or depend on an upstream webhook. CA coordinates leases and metadata through a community-bound `cai_` token; private bodies move directly through SIU artifact storage.
- **State:** The earlier CA claim that open-source Meetily provides a self-hosted bot, scheduling API, and transcript retrieval API is corrected. The adapter portion of A6 is now shaped. R5 remains failed only because the JaaS pilot records on 8x8 infrastructure rather than a community-hosted recorder.
- **Next:** Decide whether production replaces JaaS recording with self-hosted Jitsi/Jibri or explicitly relaxes R5, then open implementation issues from the slices.

## 2026-08-11 — V1-V8 promoted into implementation slices
- **Done:** Added `docs/plans/unconference-platform-slices.md` with eight vertical slices, repository ownership, breadboard affordances, demos, acceptance criteria, dependencies, and cross-slice invariants.
- **Decisions:** Infrastructure lands inside the first slice that visibly exercises it rather than as a foundation project. The private JaaS pilot may proceed through V8. V6 carries an explicit production hold until recording moves to community infrastructure or R5 changes.
- **State:** The shape is ready for per-repository issue breakdown without pretending the production recorder gap is closed.
- **Next:** Create linked implementation issues for V1-V8 and begin with the Harmonica project/brain round-trip in V1.

## 2026-08-11 — Cross-repository implementation issues opened
- **Done:** Opened the [cross-repository tracker and SIU transcript-infrastructure issue #16](https://github.com/Citizen-Infra/social-internet-unconference/issues/16), linked to [Harmonica #659](https://github.com/harmonicabot/harmonica-web-app-pro/issues/659), [community-admin #135](https://github.com/Citizen-Infra/community-admin/issues/135), [Avails #178](https://github.com/Citizen-Infra/avails/issues/178), and [scenius-digest #17](https://github.com/zhiganov/scenius-digest/issues/17).
- **Decisions:** Keep one implementation workstream per owning repository and use SIU #16 for V1-V8 sequence, cross-system dependencies, the Meetily-derived worker, artifact infrastructure, and the production recorder hold.
- **State:** Shaping, both spikes, slices, and implementation handoffs are published. No implementation slice has started yet.
- **Next:** Start V1 in Harmonica #659 with the immutable project type and GitHub brain round-trip.

## 2026-08-11 — Meetily rejected; Deepgram pilot and Voxtral evaluation selected
- **Done:** Replaced the proposed Meetily-derived worker with a provider-neutral SIU transcription adapter. Added `spike-transcription-provider.md`, retained a concise rejected-Meetily record, and updated the shape, V6 slice, Jitsi handoff, and production hold.
- **Decisions:** Use Deepgram Nova-3 for pilot prerecorded transcription because it provides a supported API, asynchronous callbacks, word timestamps, utterances, diarization, and speaker confidence. Evaluate hosted Voxtral Mini Transcribe V2 and the exact Apache-2.0 open-weight Voxtral deployment against the same recordings before production. Speaker labels remain recording-local pseudonyms.
- **State:** Meetily is no longer an infrastructure dependency. The pilot now has two hosted processors, JaaS and Deepgram, so R5 remains failed until production self-hosts both functions or explicitly relaxes the requirement.
- **Next:** Update implementation issues, run the provider bake-off during V6, and start V1 in Harmonica #659.

## 2026-08-11 — Transcription implementation handoffs updated
- **Done:** Updated community-admin #135 and SIU tracker #16 to remove the Meetily-derived worker, add the provider-neutral Deepgram adapter, preserve pseudonymous diarization, and include the Voxtral bake-off and expanded R5 production hold.
- **State:** Planning documents and implementation issues now agree. Meetily appears only in the rejected-option record and historical process notes.
- **Next:** Start V1 in Harmonica #659; execute the provider bake-off when V6 has consented test recordings.

## 2026-08-11 — Streamplace main-event broadcast captured
- **Done:** Verified hosted Streamplace AT Protocol sign-in, RTMPS/WHIP ingest, public announcement flow, and outbound multistreaming. Added a dedicated broadcast spike, linked it from the shape, added a parallel workstream to the slices, and corrected the README's unverified direct-MiroTalk and shared-identity claims.
- **Decisions:** Streamplace is the candidate public broadcast/discovery layer, not a room, lobby, attendance source, or private artifact store. It remains outside V1-V8. An SIU-controlled broadcaster identity and explicit organizer consent are required; ingest credentials remain secret.
- **State:** The hosted ingest contract is credible, and SIU issue #2 now tracks the corrected technical and relationship decisions. MiroTalk room composition, operator failover, capacity, and self-hosted Streamplace remain open selection gates.
- **Next:** Run a consented MiroTalk-to-compositor-to-Streamplace rehearsal after the main-event room and scale decisions.

## 2026-08-11 — Canonical brain directory clarified
- **Decision:** `unconference-brain/` is a first-class directory inside the `social-internet-unconference` repository, not a separate repository. The SIU repo is a single canonical planning-and-record container; we will not call it a monorepo unless it later contains independently versioned or buildable packages.
- **Impact:** Harmonica connects the SIU repository and selects the `unconference-brain/` directory as its managed container. V1-V8 references now use this model.

## 2026-08-11 — Atmospheric Groups interoperability pilot scoped
- **Done:** Opened [SIU #17](https://github.com/Citizen-Infra/social-internet-unconference/issues/17) as the public cross-repository checklist for the Atmospheric Groups interoperability pilot. Harmonica's consume-and-publish work and Community Admin's Atmospheric Groups work remain tracked in their owning repositories.
- **Decisions:** Treat SIU as a concrete but non-blocking interoperability pilot. Harmonica consumes portable community identity and membership and publishes public-safe deliberation references; it does not become the identity provider. Community Admin remains the Concierge/permission boundary, and the GitHub brain remains canonical for topic and session meaning. Event attendance, session invitation, agenda participation, and standing community membership are distinct relationships.
- **State:** The central identity question remains open by design: SIU may be its own DID-bearing group, an event owned by another group, or a collaboration spanning several groups. No unsettled Atmospheric Groups lexicon will be implemented merely to support the current launch.
- **Next:** After agenda distribution, map the existing Community Admin-specific bridge to the community-DID target and decide the event-versus-community identity model before implementation.

## 2026-08-12 — SIU event identity shape selected
- **Done:** Added `docs/plans/atmospheric-groups-pilot-shaping.md` and `docs/plans/spike-atmospheric-event-identity.md` for SIU #17.
- **Decisions:** SIU gets a dedicated event DID. Organizers govern it through scoped, revocable roles; it is not a standing participant community. Anyone can complete the Harmonica proposal flow. Harmonica then points to the SIU Bluesky profile, and a verified follow grants an event-scoped participant relationship whose initial capabilities are proposal support and SIU standing availability. Following does not grant group membership, organizer authority, room entry, or transcript access.
- **State:** The product model is selected. DID provisioning, follow-grant lifecycle, and the public-safe publication envelope remain flagged protocol mechanics for the spike. The live launch remains independent.
- **Next:** Complete the protocol spike, update the fit check, then breadboard Shape D before slicing implementation.

## 2026-08-12 — Follow-derived event participation settled
- **Decision:** Community Admin verifies one participant DID against the SIU DID through unauthenticated `app.bsky.graph.getRelationships`; bulk follower-list reconciliation is unnecessary for the participant path. The resulting event-participant grant is continuously derived with a bounded TTL.
- **Lifecycle:** Unfollow preserves support and availability evidence but those records stop qualifying for readiness. Either-direction blocks and organizer exclusion deny participation capabilities. Organizer exclusion is a durable tombstone and is not silently reversed by re-follow.
- **Impact:** The follow-to-participant mechanism now passes the shaping fit check. DID provisioning/custody and the public-safe publication envelope remain open spike work.

## 2026-08-12 — Organizer-controlled event DID bridge selected
- **Decision:** The SIU Bluesky account DID is the event identity, controlled and recoverable by organizers. For the first pilot, organizers publish proposal posts manually and register their AT URIs/CIDs in Community Admin. Community Admin gets no SIU app password; its current app-password integration grants whole-repo write access and is broader than the target scoped Concierge authority.
- **Publication:** Proposal posts point to the canonical public agenda/topic. `unconference-brain/` remains authoritative for lifecycle and meaning; outcome replies point to canonical successors, scheduled sessions, or results. No new lexicon or DID-document `#concierge` claim is required for the bridge.
- **State:** Shape D now passes its fit check. The current bridge is mapped in the protocol spike; breadboarding is next.

## 2026-08-12 — SIU event-participation breadboard completed
- **Done:** Breadboarded the open Harmonica proposal path, optional Bluesky follow, proposal-like support, Avails authorization, organizer exclusion, and readiness wiring in `docs/plans/atmospheric-groups-pilot-shaping.md`.
- **Correction:** Removed the earlier cross-surface support assumption from the main platform shape and slices. Harmonica forms proposals; it does not collect V3 support. Support comes from Bluesky liker DIDs with active follower-derived SIU event-participant grants, and Avails uses that same grant for SIU standing availability.
- **State:** The platform shape, slices, #17 shape, and bridge spike now agree. The next step is to slice the breadboard into the smallest demoable implementation increment.

## 2026-08-12 — Atmospheric Groups pilot sliced
- **Done:** Added `docs/plans/atmospheric-groups-pilot-slices.md` with three vertical increments: AV1 event DID plus follower-qualified support, AV2 shared grant authorization for Avails, and deferred AV3 portable publication plus scoped Concierge delegation.
- **Decisions:** AV1 replaces the membership-specific identity portion of platform V3 and must include unfollow, block, and organizer-exclusion behavior in its first demo. AV2 replaces the membership-specific authorization portion of V4. AV3 is standards-dependent and not part of the launch bridge.
- **Next:** Create per-repository implementation issues or plans for AV1, starting with Community Admin's SIU DID binding, manual post registration, relationship grant, and qualifying tally contract.

## 2026-08-12 — Approval-triggered publication, card support, and session links
- **Decisions:** Steward approval in Harmonica triggers auto-publication: Harmonica calls a narrow Community Admin publish endpoint, and CA posts from the SIU Bluesky account using its existing per-community app-password pattern (only CA holds the credential; organizers keep account custody; scoped delegation replaces this in AV3). This revises the earlier manual-posting bridge. Topic cards on `/p/[slug]` show the qualifying supporter count with a "Support on Bluesky" deep-link rather than collecting votes inline; inline OAuth likes remain a later option. Scheduled session cards render the stable CA join URL plus an auto-generated Excalidraw whiteboard link (random room id + key in the URL fragment; bearer link, public display intended for this open event).
- **Impact:** Rippled through `unconference-platform-shaping.md` (R2, decisions, A3/A5, breadboard U8/U10/N7/N18), `unconference-platform-slices.md` (V3/V5), and the Atmospheric Groups pilot shaping, spike, and slices.

## 2026-08-13 — Kaliya Young input and public README correction
- **Done:** Replaced the July private-planning README with the current public event, participation, architecture, V1-V8 status, privacy boundary, and open decisions. Added `docs/research/2026-08-13-kaliya-young-advisor-input.md` from Kaliya's direct messages, Project Weave, and her recent protocol articles.
- **Decisions:** Frame SIU around one calling question and invitation, not conference tracks. Treat QiQo as tested online Open Space prior art to review, not a selected platform. Define Atmospheric Groups interoperability behaviorally: SIU must be able to replace or bypass an application while retaining identity, relationships, agenda, approved public records, and collective knowledge. The event DID bridge is a bounded subset of a first-class group object, not proof of a complete Verifiable Trust Community.
- **Scope:** The calling-question correction affects launch. The independent consumer/exit test and three-layer impact review are post-launch proof and do not block agenda distribution.
- **Next:** Walk through QiQo layouts with Kaliya; settle the calling question and invitation; run the fresh V2 lifecycle preview; implement AV1; later exercise the exit test before claiming interoperability.

## 2026-08-13 — V2 complete lifecycle validated
- **Done:** Ran a fresh synthetic participant through the live proposal session with two topics: shaped and confirmed topic one, accepted continuation, shaped and confirmed topic two, declined a third, and received an explicit question-free closing turn. The stored thread became inactive.
- **Result:** V2's multi-topic lifecycle and finality gate passes. SIU #16 now marks V2 complete.
- **Residual:** Four of eight assistant turns used duplicate or compound question formulations despite the one-question rule. The stored transcript proves this is not MCP rendering. It is a facilitation-quality defect, not a V2 finality blocker.
- **Next:** Set the calling question/invitation and distribution channel, then implement AV1 across Community Admin and Harmonica.

## 2026-08-13 — QiQo walkthrough prepared
- **Done:** Added `docs/research/2026-08-13-qiqo-walkthrough-brief.md`, grounded in QiQo's online-unconference guide, current IIW format, and IIW's QiQo-bound notes process.
- **Focus:** Ask Kaliya to demonstrate arrival, live agenda creation, room movement, notes, operational roles, and exit/export behavior. Capture Keep/Adapt/Avoid/Unknown rather than taking a generic product tour.
- **Decision boundary:** QiQo is tested prior art, not a selected platform. The walkthrough should end with at most three SIU layout options and one rehearsal recommendation.
- **Next:** Run the walkthrough, record Kaliya's corrections, then settle the synchronous lobby/room shape in issue #6.

## 2026-08-13 — SIU event identity provisioned
- **Done:** Created [`unconference.bsky.social`](https://bsky.app/profile/unconference.bsky.social), resolving to `did:plc:mzvqnxye3oejamuwmfl4qvou`. This closes the event-identity provisioning prerequisite in Shape D.
- **Boundary:** The handle is public discovery; the DID is the stable binding key. For SIU standing availability, Community Admin verifies the Bluesky follow and exposes the resulting scoped relationship through its membership contract; Avails consumes that CA membership and does not check Bluesky directly. CA follow-derived membership is tracked in community-admin#136.
- **State:** Profile live with no proposal posts yet. Community Admin still needs the account app password under `CA_SERVICE_ACCOUNTS` before AV1 can auto-publish.
- **Next:** Configure the CA service account securely, then implement and test AV1 publication plus follower-qualified support.

## 2026-08-13 — AV1/AV2 execution checkpoint
- **Session:** OpenCode `ses_00fe2c0d4fferrMs11kZk5vuBI`.
- **Verified:** Railway remote MCP is installed and authenticated. Community Admin and Avails production both carry the variables needed for their trusted membership seam (`CA_CONFIG_SECRET` plus Avails' `CA_MEMBERSHIP_URL`); OAuth access exposes variable names, not secret values.
- **Production state:** Avails' public `/api/communities` response currently contains only `cibc`, so SIU is not yet selectable there. Avails consumes CA memberships; it must not verify Bluesky follows directly.
- **Tracking:** SIU #17 is the public cross-repository checklist. AV1 implementation remains split between Community Admin and Harmonica. Avails#178 owns later readiness; the AV2 event-grant consumer contract still needs implementation planning before assigning it to an Avails issue.
- **Correction:** Community-admin#136 grants ordinary CA membership from follows and is not an SIU dependency. SIU deliberately uses the separate `event_participants` bridge in community-admin#135 so following grants event-scoped capabilities rather than standing membership. Avails must consume that event grant in AV2, not infer SIU participation from ordinary CA memberships.
- **Next:** Implement AV1 from the existing plan under community-admin#135, starting with migration 022 and the follow-grant verifier. Configure the SIU service credential only when the publish path is ready for an end-to-end test.

## 2026-08-17 — Kaliya call written up, and six artefacts name-corrected
- **Done:** Added `docs/research/2026-08-14-kaliya-call-siu-format.md` — two experienced open-space facilitators independently questioning the mechanic SIU is built on, plus Kaliya's format prescription and her 50-person floor. Commented Robin onto #7 as a co-organiser candidate. Corrected across #18, #19, #23, #13, #7 and the mirrored topic records: **QiQo** (not Kiko), **Roomy** (not Rumi), **Metagov** (not MetGov), **Tracy Kunkler** (not Tracey Rogers Brandt), and removed an unverifiable founder name.
- **Decisions:** The critical-mass problem (#24's 50-person floor), the invite list (#5) and co-organisers (#7) are one problem — 50 people cannot come from one person's network, so co-organisers *are* the attendance answer. Topic-record fixes applied to the Harmonica DB as well as the files, since `detail` is the round-trip source.
- **State:** Clean and pushed. Every remaining bad-name grep hit sits inside a correction footer, verified.
- **Next:** The recording-norm question (#23) is still unanswered and bounds what SIU captures. Robin has not been asked anything yet.

## 2026-08-20 — AV2 producer/consumer contract shaped
- **Done:** Traced the shipped Community Admin event-grant producer and Avails' current standing-availability, `ca-community`, and booking paths. Added `docs/plans/siu-av2-event-grant-consumer-contract.md` with the exact introspection payload, event-DID scope, five-minute source TTL, fail-closed behavior, revocation semantics, and no-Bluesky-query invariant.
- **Decisions proposed for review:** Add a distinct `ca-event` scope keyed by the SIU DID; introspect Community Admin on every event-scoped write; retain but stop qualifying records after grant loss; and add a read-only three-person overlap operation rather than reuse side-effecting `schedule_call`.
- **Privacy consequence:** Current Avails records live publicly in each participant's PDS. The public SIU agenda would expose aggregates only, but AV2 cannot claim the underlying record is private. Review must accept explicit disclosure or select a larger private-storage shape.
- **State:** No AV2 implementation has started. The contract's review gates must close first.
- **Next:** Review the six gates in the AV2 contract, then create the Community Admin and Avails implementation handoffs without collapsing standing availability and supporter-overlap readiness into one action.

## 2026-08-20 — AV2 contract approved
- **Decision:** Approved all six contract gates: `ca-event` keyed by SIU DID, five-minute source TTL, online fail-closed introspection, immediate exclusion with retained participant records, read-only three-person overlap, and participant-owned public PDS availability with explicit disclosure.
- **State:** AV2 implementation is unblocked. Community Admin producer work and Avails consumer/readiness work remain separate implementation actions.
- **Next:** Implement Community Admin's event-grant introspection and shared freshness semantics.
- **Next:** Implement Avails' SIU event-scoped standing-availability write gate.
- **Next:** Implement Avails' read-only three-supporter overlap readiness operation without automatic scheduling.

## 2026-08-20 — AV2 implementation opened for review
- **Community Admin:** [PR #142](https://github.com/Citizen-Infra/community-admin/pull/142) adds stable event-DID bindings, capability-specific online introspection, shared five-minute grant freshness, immediate exclusion, and fail-closed support behavior.
- **Avails:** [PR #179](https://github.com/Citizen-Infra/avails/pull/179) adds `ca-event` records, online write authorization, the SIU participant editor, and service-only read-only three-person overlap. `schedule_call` rejects event scopes.
- **Verification:** Avails passed 58 focused dependency-free tests, syntax checks, diff checks, and the UI detector. Community Admin passed syntax and diff checks. Dependency-backed Community Admin tests, Avails Express route integration, and the Avails client build remain for CI because neither checkout had installed dependencies and none were installed for this task.
- **State:** PRs are the stopping point. Community Admin must deploy before Avails enables the consumer path.
- **Next:** Review and merge Community Admin PR #142.
- **Next:** Review and merge Avails PR #179 after confirming its producer dependency and SIU editor path.

## 2026-08-20 — AV2 implementation merged
- **Community Admin:** PR #142 merged as `a86c8c20202f18113bc2f3080027248707133c5f`; all `server`, `admin`, and `clients` CI checks passed.
- **Avails:** PR #179 merged as `0ac73f55930cfe99285abcd15a856c023c4f30f8`; the `build` CI check passed.
- **Review state:** Both PRs were clean with no review threads or requested changes. They were squash-merged in producer-then-consumer order.
- **State:** AV2 code is merged. Deployment monitoring and production validation were not requested.
- **Next:** Implement Community Admin's readiness orchestration: supply only active proposal-supporter DIDs to Avails' read-only overlap operation and project aggregate readiness without automatic scheduling.

## 2026-08-20 — AV2 readiness orchestration opened for review
- **Decision:** While SIU has no fixed date, evaluate 60-minute sessions over the next 56 calendar days, matching Avails' default standing-availability lifetime.
- **Implementation:** [Community Admin PR #143](https://github.com/Citizen-Infra/community-admin/pull/143) supplies only active proposal-supporter DIDs to Avails' `evaluate_availability_overlap`, stores aggregate readiness, and exposes `needs-support`, `needs-availability`, or `ready` without calling `schedule_call`.
- **Failure behavior:** Missing configuration, malformed results, and Avails outages cannot leave a topic ready; no booking, email, invitation, poll, or calendar side effect is available on this path.
- **Verification:** Syntax and diff checks passed; focused database-backed tests were added but remain for CI because the local checkout has no installed server dependencies.
- **Merged:** PR #143 passed `server`, `admin`, and `clients` CI and was squash-merged as `7b5e2dc6c69c6acbd5f694a47b5930444954a463`.
- **Correction:** This merge completes the Community Admin caller and aggregate storage, not the full readiness projection. Harmonica must still render `needs support`, `needs availability`, or `ready` without exposing supporter identities or individual windows.
- **Next:** Implement the Harmonica aggregate readiness projection.
- **Next:** Confirm Community Admin and Avails deployment state and production configuration before validating SIU writes, revocation, restoration, and shared three-person overlap.
- **Next:** Run the retrospective audit of the five Harmonica SIU pull requests and their combined behavior on current `main`.
- **Next:** Resolve SIU #8's reflection timing, session closure, and synthesis publication design.
- **Next:** Complete the deferred AV3 proof with an independent consumer, application replacement, and scoped Concierge delegation.

## 2026-08-24 — SIU is a neutral container (#25 answered and closed)
- **Decision:** SIU convenes the ATProto and social-internet ecosystem, not CIBC. It produces no commitments of its own; harvest is a book of proceedings; CIBC's job is to hold the space. Kaliya's IIW phrasing applies: "it's not our job." Chosen against the recommendation on file, which argued that #17 and the AV2 investment already made it CIBC convening for itself.
- **Three consequences, all now open:**
  - **#17 conflicts as written.** Using the event as CIBC's Atmospheric Groups interoperability pilot puts a CIBC objective inside a container just declared neutral. Two viable shapes: an invisible pilot that asks nothing of participants, or drop it from SIU and test on a CIBC-only gathering. Asking participants to adopt CIBC tooling so the pilot has data is not viable.
  - **Attendance cannot require CIBC's stack (#13).** AV2 requires an ATProto identity, a Community Admin event grant keyed by the SIU DID, and an Avails standing-availability record. A neutral container needs a path in for someone with none of those, and AV2 has no such fallback. PRs #142, #179 and #143 all assume the grant path, so retrofitting gets more expensive with each layer.
  - **The convening purpose is now blocking (#3).** #25 already recorded that Artem's honest motivation is "not yet a convening purpose." A gathering for itself can run on the convenor's motivation; a neutral container cannot. #3 must be answered before any invitation goes out.
- **Downstream:** #8 narrows to session notes and proceedings — cross-session synthesis, action items and owners are out of scope, and "synthesis publication" should be redesigned as proceedings publication. #1 and #3 should both read as an ecosystem invitation. #26 unchanged; neutrality is not a licence to modify open space.
- **Easier under this choice:** the 50-person floor (#24), because "come to the ecosystem's event" recruits better than "come to CIBC's"; co-organisers (#7) become genuine co-hosts; professional facilitation is worth more, since holding a neutral space is the skill.
- **State:** #25 closed. Comments left on #17, #13 and #8. No code changed.
- **Next:** Answer #3's calling question, now blocking the invitation.
- **Next:** Choose the #17 shape — invisible pilot or drop from SIU.
- **Next:** Design the AV2 no-ATProto fallback before more AV2 work lands.

## 2026-08-24 — Tracy Kunkler session processed, three days late
- **Source:** 2026-08-21, ~90 min with Tracy Kunkler and Graham, captured by Graham's ChatGPT notetaker. No Fathom or Fireflies record exists. The transcript had been sitting **untracked in this public repo's working tree** since 08-22; moved to `cibc-brain/docs/research/2026-08-21-tracy-kunkler-open-space-walkthrough.md`, which is where raw call material belongs.
- **Consent:** given by everyone. Agreed before Graham started recording, so it is absent from the transcript. A first pass read the tape alone and recorded Tracy as not consenting, which was accurate about the transcript and wrong about the call. A recording cannot testify to what happened before it started.
- **Headline:** Artem stated a real convening purpose and said outright it is not what he told Kaliya. He and Erlend both want to know how open source developers fund themselves without venture capital. Tracy turned it into a theme in about a minute — how do we resource ourselves when we tell the venture capitalists no — and proposed philanthropists and developers in one room under a no-pitching, nobody-asks-for-money ground rule. Commented to #3. This closes the gap #25 named.
- **Second headline:** Tracy suggested letting go of the unconference format for now, and testing standing availability through CIBC's existing town hall as a community of practice instead. Purpose drives process; a community of practice reaches the Avails experiment without 50 strangers or a five-hour block. Not yet filed as its own issue.
- **Decisions supported, none taken:**
  - **#19 / #13** — the agenda chaos is a feature, and Tracy's constraint is narrow and testable: software may propose merges and moves, the named proposer decides. Two facilitators independently on the same point now.
  - **#7** — the diffusion problem has a prescription. A core team of about five, committed to each other, recruited *after* the purpose is sharp. Artem said plainly that his WhatsApp and Signal groups are silent and he is not sure the event will happen.
  - **#24** — Alaska alliance runs this at 90 minutes every other month, down from two hours. Artem argued against his own five-hour option. The middle option costs the Avails test.
  - **#23** — notetakers generally cannot join breakout rooms. The norm question and the capability question have to be settled together. Session notes in a per-room doc are a separate mechanism that needs no recording.
  - **#26** — the facilitator role is "unfacilitated"; session hosts are not experts; butterflies need a hallway space built or the role has nowhere to happen; the law of mobility needs scripting in the opening.
  - **#1** — Graham finds "unconference" uninviting on two counts: it reads as lesser, and it still signals conference-scale commitment.
- **Precedent worth keeping:** Ben Roberts' hybrid dialogue inquiry is a prior failure of SIU's async model — survey, eight topics, eight WhatsApp chats, nobody talked. Artem's diagnosis was that people did not know each other. It is also where he and Graham met.
- **Not for this repo:** Graham does not understand how Harmonica works and wants Polis-like functionality for his team of five or six. A positioning finding for the Harmonica side.
- **State:** Extraction committed and pushed to cibc-brain. Comments on #3, #7, #19, #24, #23, #26, #1. No code changed, no issue closed.
- **Next:** Sharpen #3's purpose into a six-word question, since #7 depends on it.
- **Next:** Decide whether the community-of-practice route replaces or precedes the unconference.
- **Next:** Verify whether any candidate platform can put a notetaker inside a breakout room.

## 2026-08-24 — Tracey Rogers Brandt is a real person; the 08-16 name sweep merged two people
- **What was wrong:** the 2026-08-16 correction sweep recorded "Tracey Rogers Brandt" as a Fathom invitee-list artefact "appearing in no other record" and treated her as Tracy Kunkler. Artem was asked and confirmed "same person", which is now read as confirming the *speaker*, not the identity.
- **What is true:** Tracey Rogers Brandt is at **Circular Patterns**, El Cerrito California, and appears alongside **She's Geeky** — the unconference Kaliya Young founded. She sits in Kaliya's network. She was genuinely invited to the 2026-08-14 call and did not speak. The speaker is **Tracy Kunkler** of Circle Forward, `tracy@circleforward.us`. Two people.
- **Why it was missed:** "appears in no other record" was true of the local archives and nobody searched outside them. A single web search settled it. **Absence from our own files is not evidence a person does not exist** — it is evidence we have never recorded them.
- **Corrected in:** both brains, the SIU research capture, the Pro extraction file, and this note.
- **Next:** correct the SIU issues where the 08-16 sweep applied the merge (#18, #19, #23, #13, #7).

## 2026-08-24 — #17 shape decided: invisible pilot confirmed, exit test moves to the town hall
- **Decision:** the Atmospheric Groups pilot stays in SIU as an **invisible pilot**. No design change was needed, because that is already what #17 decided: *"Anyone can complete the Harmonica topic-proposal flow without joining or using Bluesky."* A follow is optional and grants only support-qualification and SIU standing availability. The #25 neutrality conflict is closed.
- **Two corrections to comments made earlier the same day, both mine:**
  - The #17 comment framed "invisible pilot" as something still to be designed and offered dropping the pilot as the alternative. It was already the design, and dropping it would now strand AV1 in production plus merged AV2 work.
  - The #13 comment said proposing or scheduling a session requires an ATProto identity, a CA grant and an Avails record. **Proposing requires none of them.** The grant gates support-qualification and standing availability. The real gap is narrower: a non-ATProto participant can put a topic on the agenda but cannot help anything reach the support threshold, including their own.
- **What moves:** #17's behavioral interoperability exit test — replace or bypass one application while retaining identity, relationships, agenda, records and knowledge — **runs at the CIBC town hall rather than waiting on the public event.** It needs a real group using the stack, not 50 strangers, and the 08-24 community-of-practice decision puts the town hall first. Better test bed on two counts: participants are known, so a failure can be debugged with the person it happened to; and CIBC members on CIBC infrastructure raise none of the questions #25 settles.
- **Unchanged:** the constraint that the interop work must never grow an ask of participants in order to have something to measure. Holds whatever the venue.
- **State:** comments on #17 and #13. No code changed, no acceptance box ticked. Only the venue for the last two changed.
- **Next:** the AV2 non-ATProto support/availability gap now blocks the public event rather than the next thing that happens. Solve it before more AV2 lands on top.
