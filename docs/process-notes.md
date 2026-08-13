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
- **Done:** Opened [SIU #17](https://github.com/Citizen-Infra/social-internet-unconference/issues/17) and Harmonica child issue [HAR-1546](https://linear.app/harmonica-pro/issue/HAR-1546), linked under the existing consume-and-publish direction in [HAR-1423](https://linear.app/harmonica-pro/issue/HAR-1423) and Community Admin's Atmospheric Groups epic [#56](https://github.com/Citizen-Infra/community-admin/issues/56).
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
