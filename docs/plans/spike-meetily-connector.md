---
shaping: true
---

# Spike: SIU-Hosted Meetily Connector

**Status:** Complete; SIU-owned headless adapter selected
**Date:** 2026-08-11

## Context

CA's 2026-07 integrations design names Meetily as connector #2 and claims a
self-hosted bot, scheduling API, and transcript retrieval. Those claims do not
describe the supported open-source application at v0.4.0. Community Meetily is
a self-contained Tauri desktop app. Its old Python/FastAPI and Docker backend is
archived, unsupported, unauthenticated, and explicitly unsuitable for new
production deployments.

## Goal

Identify the deployable adapter around Meetily and its narrow CA connector
contract, including private artifact storage, access, retries, and outputs for
the brain, digest, and authorized tools.

## Findings

| Claim or need | Verified state at v0.4.0 | Consequence |
|---|---|---|
| Deployable edition | Community v0.4.0 at commit `0281737d87d26352fb0adc78c8c0975f691b23d1` is MIT licensed. Pro is a separate codebase and its server contract is not publicly verifiable. | Pin the open-source commit and record local patches. Do not assume Pro capabilities. |
| Supported architecture | Next.js calls a Rust core through Tauri commands and events. Local SQLite and meeting folders hold results. | There is no supported headless server process to deploy unchanged. |
| Audio import | The beta `start_import_audio_command` accepts a local path, title, language, model, and provider. It emits progress/completion events and writes timestamped transcript segments. | The reusable seam is internal Rust code, not HTTP. |
| Concurrency | The import path uses a process-global in-progress guard. | Pilot with one transcription at a time per worker. Scale with isolated workers, not concurrent calls in one process. |
| API and Docker | The FastAPI API and Docker files under `backend/` are a legacy archive. Upstream says they are unsupported and had unauthenticated, development-oriented CORS behavior. | Never expose or revive the archived API. |
| Bot and scheduling | Community Meetily records desktop microphone/system audio. It has no supported Jitsi bot or meeting scheduling API. | JaaS records. The adapter begins with an already-ingested private media artifact. |
| Webhooks | Generic transcript/summary webhooks are still open issue #595. | Use CA's connector pull/lease/result convention; do not wait for an upstream webhook. |
| Transcript | Import writes `transcripts.json` plus SQLite rows with text, start, end, and duration. | Normalize those timestamps into the community transcript schema. |
| Speakers | Community v0.4.0 import does not diarize or identify speakers. Meetily advertises speaker diarization for Pro. | Preserve speaker fields as `null`; do not infer speakers from JaaS attendance. |
| Summary/export | The desktop app can generate summaries and export transcript text, but these remain internal Tauri/UI flows. | The worker may reuse summary code later; transcript production must not depend on UI export automation. |

## Decision

Build an SIU-owned headless connector that extracts or refactors the minimum
MIT-licensed Community Meetily import pipeline into a worker binary. Keep the
upstream version and commit in every result. Do not run the archived FastAPI
backend, automate a desktop window, or present the adapter as an upstream
supported Meetily server.

The first worker reuses decoding, VAD, Whisper/Parakeet transcription, and
timestamped segment generation. It does not join Jitsi, start recordings, grant
transcript access, publish results, or infer speakers. Those responsibilities
stay with JaaS, SIU artifact storage, CA, and organizer review.

## Deployable Components

| Component | Responsibility |
|---|---|
| CA Meetily integration | Community-scoped job state, leases, event linkage, progress, result metadata, and `cai_` authentication. |
| SIU connector worker | Poll CA, resolve private media from SIU storage, run the pinned Meetily-derived pipeline, normalize output, and report artifact references. |
| SIU artifact service | Store encrypted media/transcripts, verify hashes, enforce lifecycle deletion, and mint short-lived reads after CA authorization. |
| CA transcript gate | Check booked-invitee or verified-attendee ACL before requesting a short-lived transcript URL. |
| CA transcript processor | Create reviewable synthesis and literal-URL candidates from the private transcript without publishing them. |

The worker image includes the model runtime but no GitHub, JaaS, or calendar
credentials. Its CA `cai_` token and SIU storage credential are mounted at run
time and scoped to one community.

## Connector Contract

The connector follows CA's existing pull pattern through kind-specific routes:

```text
POST /communities/:communityId/connector/ping
GET  /communities/:communityId/connector/meetily/work
POST /communities/:communityId/connector/meetily/work/:jobId/progress
POST /communities/:communityId/connector/meetily/work/:jobId/result
```

Every route requires `Authorization: Bearer cai_<secret>`. CA resolves the hash
to one active integration and rejects a community or connector-kind mismatch.
There is no connector callback URL and no transcript body passes through CA.

CA returns `204` when no work is available. A claimed job has this shape:

```json
{
  "job_id": "<stable-job-id>",
  "lease_expires_at": "<ISO-8601>",
  "community_id": "<community-id>",
  "event_id": "<ca-event-id>",
  "session_id": "<stable-session-id>",
  "input": {
    "artifact_id": "<private-media-id>",
    "sha256": "<expected-hash>",
    "media_type": "video/mp4",
    "bytes": 0,
    "source_expires_at": "<ISO-8601>"
  },
  "transcription": {
    "language": "en",
    "provider": "whisper",
    "model": "<pinned-model-id>"
  },
  "retention": {
    "media_delete_at": "<ISO-8601>",
    "transcript_delete_at": "<ISO-8601-or-null>"
  }
}
```

`bytes` contains the actual expected byte count. Zero above is only a schema
placeholder. The connector resolves `artifact_id` directly against the SIU
artifact service using its local storage credential, then verifies byte count
and SHA-256 before transcription.

## State And Idempotency

CA owns `pending`, `leased`, `processing`, `completed`, `retryable_failed`,
`terminal_failed`, and `deleted`. Progress reports identify one of
`downloading`, `decoding`, `transcribing`, `summarizing`, or `uploading` and may
renew the lease.

`job_id` is the idempotency key. The worker uses an isolated workspace named by
that ID. If the transcript artifact and matching input hash already exist, it
reports the existing result instead of transcribing again. CA accepts the first
valid terminal result and returns success for exact duplicates. A conflicting
artifact hash is an error requiring organizer review.

Retry network, lease, temporary storage, model-load, and capacity failures with
bounded exponential backoff. Treat an input hash mismatch, unsupported media,
invalid model, or expired/deleted source as terminal. Alert before the JaaS
source URL or SIU input lifecycle expires.

## Result Contract

The worker uploads artifacts directly to SIU storage, then reports metadata:

```json
{
  "status": "completed",
  "job_id": "<stable-job-id>",
  "transcript": {
    "artifact_id": "<private-transcript-id>",
    "sha256": "<transcript-hash>",
    "schema_version": "siu.transcript.v1",
    "segments": 0,
    "duration_ms": 0
  },
  "draft_summary": {
    "artifact_id": "<private-draft-id-or-null>",
    "sha256": "<draft-hash-or-null>"
  },
  "engine": {
    "adapter_version": "<siu-adapter-version>",
    "meetily_version": "v0.4.0",
    "meetily_commit": "0281737d87d26352fb0adc78c8c0975f691b23d1",
    "provider": "whisper",
    "model": "<model-id>"
  }
}
```

Counts and durations contain actual values. CA verifies the artifact metadata
with the SIU artifact service before marking the job complete.

## Transcript Schema

The private artifact is UTF-8 JSON:

```json
{
  "schema": "siu.transcript.v1",
  "community_id": "<community-id>",
  "event_id": "<ca-event-id>",
  "session_id": "<stable-session-id>",
  "source_sha256": "<media-hash>",
  "segments": [
    {
      "sequence": 0,
      "start_ms": 0,
      "end_ms": 1500,
      "text": "Example transcript segment.",
      "speaker": { "id": null, "name": null }
    }
  ]
}
```

Segments retain Meetily's audio start/end offsets. The pilot does not claim
speaker attribution. Literal-link extraction operates on `text`; an approved
public link may cite session and time range but never a private quote or speaker.

## Storage, Access, And Deletion

- JaaS is only the temporary recording source. The ingestion worker copies its
  recording into encrypted SIU storage before the 24-hour JaaS expiry.
- The connector uses a job workspace, uploads normalized derivatives, verifies
  them, and wipes the workspace after completion.
- Every integration requires explicit `media_delete_at` and transcript retention
  policy. The pilot default is media deletion seven days after a verified
  transcript; an organizer may shorten it. No indefinite media default exists.
- Transcript deletion follows the configured community policy. Deletion removes
  the private artifact and access grants while retaining a non-sensitive job
  tombstone and hashes for audit/idempotency.
- CA stores artifact IDs, hashes, state, and ACLs, not transcript/media bodies.
- A caller asks CA for transcript access. CA checks booked invitation or verified
  JaaS attendance, then requests a short-lived, single-artifact read from SIU
  storage. Repository membership never grants private transcript access.

Approved synthesis and links are separate public-safe derivatives written to
`unconference-brain/`. Any authorized community tool may use the same CA access
gate; Harmonica receives no privileged transcript path.

## Production Smoke

1. Create one private test event with two invitees, one other community member,
   and one non-member test account.
2. Record a short consented JaaS session. Verify join webhooks add only actual
   authenticated attendees to the ACL.
3. Ingest the JaaS recording into SIU storage and verify byte count and hash.
4. Let the connector lease the job, report progress, transcribe it once, upload
   `siu.transcript.v1`, and report the pinned engine provenance.
5. Redeliver the same work and result. Verify no second transcript is created.
6. Verify invitees and joined attendees can open a short-lived transcript URL;
   the uninvolved member and non-member cannot.
7. Approve one synthesis and one URL, then verify only those derivatives enter
   the brain and My Community digest.
8. Trigger media expiry and verify the transcript remains accessible while the
   media and all temporary workspaces are gone.

## Correction To The CA Design

The 2026-07 CA statement that Meetily provides a self-hosted Docker bot,
scheduling API, and transcript retrieval API is not true of the supported
open-source v0.4.0 application. The repository contains Docker and FastAPI files
only as an unsupported legacy archive. The current Community app provides local
desktop capture and internal Tauri commands. Website references to REST APIs and
webhooks do not identify an implemented open-source server contract, and the
generic outbound webhook remains an open feature request.

Connector #2 therefore means an SIU-owned, Meetily-derived headless worker. If
the team instead wants an upstream-supported server, it must separately evaluate
and contract for Meetily Pro/Enterprise; that path is not established by this
shape.

## Acceptance Result

The spike establishes deployable boundaries, pinned reusable code, exact CA
work/result contracts, private artifact handling, access checks, retention and
deletion behavior, failure states, and an end-to-end smoke. It closes the
Meetily adapter portion of A6. R5 remains failed only because the selected JaaS
pilot records on 8x8 rather than community-hosted infrastructure.

## Sources

- [Meetily v0.4.0 release](https://github.com/Zackriya-Solutions/meetily/releases/tag/v0.4.0)
- [MIT license at the pinned commit](https://github.com/Zackriya-Solutions/meetily/blob/0281737d87d26352fb0adc78c8c0975f691b23d1/LICENSE.md)
- [Supported Tauri architecture](https://github.com/Zackriya-Solutions/meetily/blob/0281737d87d26352fb0adc78c8c0975f691b23d1/docs/architecture.md)
- [Archived backend warning](https://github.com/Zackriya-Solutions/meetily/blob/0281737d87d26352fb0adc78c8c0975f691b23d1/backend/README.md)
- [Archived API security warning](https://github.com/Zackriya-Solutions/meetily/blob/0281737d87d26352fb0adc78c8c0975f691b23d1/backend/API_DOCUMENTATION.md)
- [Community audio-import pipeline](https://github.com/Zackriya-Solutions/meetily/blob/0281737d87d26352fb0adc78c8c0975f691b23d1/frontend/src-tauri/src/audio/import.rs)
- [Open outbound-webhook request #595](https://github.com/Zackriya-Solutions/meetily/issues/595)
- [Meetily export documentation](https://docs.meetily.ai/features/export-and-copy)
- [`community-admin` integrations/connectors design](https://github.com/Citizen-Infra/community-admin/blob/main/docs/plans/2026-07-19-integrations-connectors-design.md)
