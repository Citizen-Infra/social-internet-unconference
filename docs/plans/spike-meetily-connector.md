---
shaping: true
---

# Spike: SIU-Hosted Meetily Connector

## Context

CA's 2026-07 integrations design names Meetily as connector #2 and claims a
self-hosted bot, scheduling API, and transcript retrieval. Current public
Meetily material verifies local/system-audio recording, transcription, imports,
and export, but does not provide a stable public meeting/transcript API or an
open-source webhook implementation. Hosting Meetily on the SIU server therefore
does not by itself establish how it joins Jitsi or reports a transcript.

## Goal

Identify the deployable adapter around Meetily and its narrow CA connector
contract, including private artifact storage, access, retries, and outputs for
the brain, digest, and authorized tools.

## Questions

| # | Question |
|---|---|
| M1-Q1 | Which Meetily edition, commit/version, and license will SIU deploy? |
| M1-Q2 | Does that deployment expose supported APIs for importing media, starting transcription, reading status, and retrieving transcripts/summaries? |
| M1-Q3 | If only internal APIs exist, what stable SIU-owned adapter contract isolates CA from them? |
| M1-Q4 | Does the connector record Jitsi itself, or only accept media from the recorder selected in the Jitsi spike? |
| M1-Q5 | What CA work payload schedules a recording, and which fields are required: event id, room URL, start/end, consent policy, and callback URL? |
| M1-Q6 | How does the `cai_` integration token constrain the connector to one community, and how are callbacks signed and deduplicated? |
| M1-Q7 | Where do media and transcripts live, how long are they retained, and how are they deleted? |
| M1-Q8 | How does CA authorize booked invitees and authenticated attendees without proxying transcript content unnecessarily? |
| M1-Q9 | What transcript shape preserves speaker/timestamp data while allowing public-safe synthesis and literal-URL extraction? |
| M1-Q10 | How does the connector report partial states and retryable failures such as recording missing, transcription running, or callback lost? |
| M1-Q11 | What output reaches `unconference-brain/`, what remains private, and how can tools other than Harmonica consume it? |
| M1-Q12 | What production smoke demonstrates one scheduled room becoming an accessible transcript, approved synthesis, and reviewed digest link? |

## Acceptance

The spike is complete when we can describe the deployable components, supported
Meetily calls or adapter endpoints, CA work/result payloads, artifact locations,
access checks, retention rules, and one end-to-end production verification. The
result must explicitly correct or confirm the capability claims in CA's earlier
integrations design.

## Starting References

- [Meetily](https://meetily.ai/)
- [Meetily repository](https://github.com/Zackriya-Solutions/meetily)
- [Meetily export documentation](https://docs.meetily.ai/features/export-and-copy)
- [Meetily webhook request #595](https://github.com/Zackriya-Solutions/meetily/issues/595)
- [`community-admin` integrations/connectors design](https://github.com/Citizen-Infra/community-admin/blob/main/docs/plans/2026-07-19-integrations-connectors-design.md)
