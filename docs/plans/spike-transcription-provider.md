---
shaping: true
---

# Spike: Transcription Provider

**Status:** Deepgram selected for pilot; Voxtral selected for production evaluation
**Date:** 2026-08-11

## Goal

Turn one private SIU media artifact into a timestamped, speaker-separated
transcript without coupling CA or artifact storage to one provider.

## Decision

Use Deepgram Nova-3 for the private pilot behind an SIU-owned provider adapter.
Evaluate hosted and self-hosted Voxtral against the same recordings before the
production decision.

Deepgram is the lowest-risk pilot mechanism because its prerecorded API already
accepts media bytes, supports asynchronous callbacks, and returns word timing,
utterances, diarized speaker labels, and speaker confidence. No desktop runtime
or transcription fork is required.

Voxtral is the strategic candidate because Voxtral Realtime is available as
Apache 2.0 open weights, while the hosted Voxtral Mini Transcribe V2 API offers
word timestamps and diarization at a published `$0.003/min`. Open-weight
Realtime and hosted Transcribe V2 must not be assumed to have identical batch or
diarization behavior; the bake-off must verify the deployment actually proposed.

## Comparison

| Concern | Deepgram Nova-3 | Voxtral |
|---|---|---|
| Pilot integration | Mature prerecorded API, binary/URL input, callback, structured results. | Hosted transcription API is straightforward but less proven in this stack. |
| Timestamps | Word start/end and utterance/paragraph timing. | Hosted Transcribe V2 advertises word-level timestamps. |
| Speakers | Prerecorded diarization returns speaker and speaker confidence. | Hosted Transcribe V2 advertises diarization; open-weight parity requires verification. |
| Self-hosting | Enterprise-only containers and commercial agreement. | Apache 2.0 open-weight Realtime model provides a credible community-hosted path. |
| Operations | None beyond adapter, result validation, and privacy controls. | Hosted is similar; open-weight requires GPU serving, model lifecycle, queueing, and monitoring. |
| Cost | Usage-priced; confirm the selected plan and diarization add-ons before launch. | Hosted Transcribe V2 publishes `$0.003/min`; Realtime publishes `$0.006/min`. |
| Privacy | Hosted processor; contract, region, retention, and training terms are launch gates. | Hosted API has the same class of concern; self-hosting can keep processing on SIU infrastructure. |

## Provider-Neutral Contract

CA continues to coordinate a community-scoped transcription job through its
`cai_` pull/lease/progress/result routes. CA stores no media or transcript body.
The SIU adapter resolves the input from private artifact storage and submits the
bytes to the selected provider.

```json
{
  "job_id": "<stable-job-id>",
  "community_id": "<community-id>",
  "event_id": "<ca-event-id>",
  "session_id": "<stable-session-id>",
  "input": {
    "artifact_id": "<private-media-id>",
    "sha256": "<expected-hash>",
    "media_type": "video/mp4",
    "bytes": 0
  },
  "transcription": {
    "provider": "deepgram",
    "model": "nova-3",
    "language": "<language-or-auto>",
    "timestamps": "word",
    "diarization": true
  }
}
```

`bytes` contains the actual expected byte count. The adapter verifies byte count
and SHA-256, submits media directly rather than exposing an SIU read URL, and
uses `job_id` as its idempotency key.

Provider callbacks terminate at the SIU adapter, not CA. The adapter
authenticates and deduplicates callbacks, validates the provider request/model,
normalizes the result, uploads the private transcript to SIU storage, and sends
only artifact metadata to CA.

## Transcript Schema

Every provider produces the same private artifact:

```json
{
  "schema": "siu.transcript.v1",
  "community_id": "<community-id>",
  "event_id": "<ca-event-id>",
  "session_id": "<stable-session-id>",
  "source_sha256": "<media-hash>",
  "provider": {
    "name": "deepgram",
    "model": "nova-3",
    "request_id": "<provider-request-id>"
  },
  "segments": [
    {
      "sequence": 0,
      "start_ms": 0,
      "end_ms": 1500,
      "text": "Example transcript segment.",
      "speaker": {
        "label": "speaker-0",
        "confidence": 0.98
      }
    }
  ]
}
```

Speaker labels are recording-local pseudonyms, not community identities. JaaS
attendance proves who joined but does not establish which person spoke each
segment. No UI may turn `speaker-0` into a member name without separate verified
speaker attribution.

## Pilot Gates

- Execute a data-processing agreement and verify region, retention, deletion,
  subprocessors, and model-training terms for Deepgram.
- Keep the Deepgram credential on the SIU adapter and scope/rotate it separately
  from CA and storage credentials.
- Submit media bytes; never place provider credentials or private artifact URLs
  in CA, calendars, logs, or the brain.
- Verify callback authentication and idempotency from provider documentation or
  use authenticated result polling if callback authenticity is insufficient.
- Store provider request ID, model/version, source hash, output hash, timing, and
  cost metadata for audit and comparison.

## Voxtral Bake-Off

Run at least three consented recordings representing quiet speech, overlapping
speakers, and multilingual or technical discussion. Compare Deepgram Nova-3,
hosted Voxtral Mini Transcribe V2, and the exact open-weight Voxtral deployment
under consideration.

Score:

- Proper names, organizations, URLs, and technical terms.
- Word error rate on a manually checked sample.
- Speaker error and overlap handling.
- Word/segment timestamp drift.
- Multilingual and code-switching quality.
- End-to-end latency, compute use, and cost.
- Failure recovery and reproducibility from pinned model versions.

Production selects self-hosted Voxtral only if its quality is acceptable and SIU
can operate the GPU service. Otherwise retain Deepgram under explicit approval
of the hosted-processing exception or procure a self-hosted commercial option.

## R5 Impact

The pilot does not satisfy R5's community-infrastructure clause: JaaS records on
8x8 and Deepgram transcribes as a hosted processor. Production closes R5 only by
moving both recording and transcription onto community infrastructure, or by
explicitly relaxing the requirement.

## Sources

- [Deepgram prerecorded audio](https://developers.deepgram.com/docs/pre-recorded-audio)
- [Deepgram speaker diarization](https://developers.deepgram.com/docs/diarization)
- [Deepgram pricing](https://deepgram.com/pricing)
- [Voxtral Transcribe 2 announcement](https://mistral.ai/news/voxtral-transcribe-2/)
- [Voxtral open-model announcement](https://mistral.ai/news/voxtral/)
