---
shaping: true
---

# Spike: Meetily Server Fit

**Status:** Complete; rejected
**Date:** 2026-08-11
**Replacement:** [Transcription Provider Decision](./spike-transcription-provider.md)

## Decision

Do not use Meetily as unconference infrastructure and do not derive an SIU
worker from its application internals.

Meetily is a useful local desktop product, but it is the wrong boundary for a
server-side recording pipeline. Reusing its internal Rust path would make SIU
maintain a fork without gaining a supported server API, durable job model,
authentication, webhook delivery, or speaker diarization.

## Verified Reasons

| Need | Meetily Community v0.4.0 |
|---|---|
| Headless deployment | Unsupported; the current product is a Tauri desktop application. |
| Public transcription API | None; audio import is an internal beta Tauri command. |
| Bot or meeting scheduling | None in the supported Community application. |
| Completion webhook | None; generic webhooks remain open issue #595. |
| Speaker diarization | Not implemented in the Community import path. |
| Durable multi-job service | None; import uses process-global state and one job at a time. |
| Legacy FastAPI/Docker path | Explicitly archived, unsupported, unauthenticated, and unsuitable for production. |
| License | MIT, but permissive reuse does not make application internals a stable service contract. |

## Correction To The CA Design

The 2026-07 CA statement that open-source Meetily provides a self-hosted Docker
bot, scheduling API, and transcript retrieval API is incorrect for the
supported v0.4.0 application. Pro/Enterprise is a separate codebase and no
publicly verified server contract was established in this spike.

## Sources

- [Meetily v0.4.0 release](https://github.com/Zackriya-Solutions/meetily/releases/tag/v0.4.0)
- [Supported Tauri architecture](https://github.com/Zackriya-Solutions/meetily/blob/0281737d87d26352fb0adc78c8c0975f691b23d1/docs/architecture.md)
- [Archived backend warning](https://github.com/Zackriya-Solutions/meetily/blob/0281737d87d26352fb0adc78c8c0975f691b23d1/backend/README.md)
- [Archived API security warning](https://github.com/Zackriya-Solutions/meetily/blob/0281737d87d26352fb0adc78c8c0975f691b23d1/backend/API_DOCUMENTATION.md)
- [Community audio-import implementation](https://github.com/Zackriya-Solutions/meetily/blob/0281737d87d26352fb0adc78c8c0975f691b23d1/frontend/src-tauri/src/audio/import.rs)
- [Open outbound-webhook request #595](https://github.com/Zackriya-Solutions/meetily/issues/595)
