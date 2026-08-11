# Process Notes — social-internet-unconference

## 2026-07-22 — Planning repo created from the Erlend call
- **Done:** Created the private planning repo (README + 8 issues) from the 2026-07-21 Erlend Sogge Heggen call. README marks every unsettled item as unsettled; issues cover name, Streamplace contact, themes, the Roomy space, invite list, date/format, plus two added later — co-organisers from CIBC's partners (#7) and using Harmonica for agenda-forming and session reflection (#8).
- **Decisions:** Private now, public later — nothing is named or dated, and unconfirmed contacts are named freely. Deliberately **no invented facts**: no event name, no date, no headcount, one locked theme of an implied three or four. Erlend explicitly does not coordinate; Artem drives. Split #5 (invite list) from #7 (co-organisers) once they diverged.
- **State:** Clean and pushed. Nothing has moved externally — **#2 (contact Eli about Streamplace) is unassigned and gates format, date and scale.**
- **Next:** #2 is the first real action. Check in with Erlend on the Roomy space. Approach Newspeak House as the strongest co-organiser candidate.

## 2026-08-11 — Unconference platform shaped across the community stack
- **Done:** Added `docs/plans/unconference-platform-shaping.md`, a cross-system shape and breadboard for async topic formation, reviewed deduplication, `/u/[slug]`, Bluesky support, reusable Avails, organizer-promoted scheduling, CA/My Community events, community-hosted Meetily transcription, the bidirectional `unconference-brain/`, digest-link return, and follow-up reflections. Added standalone Jitsi and Meetily spike briefs.
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
