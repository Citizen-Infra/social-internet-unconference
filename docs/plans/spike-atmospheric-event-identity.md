---
shaping: true
---

# Atmospheric Event Identity Spike

## Context

Shape D in `atmospheric-groups-pilot-shaping.md` gives SIU a dedicated event
DID while keeping organizers, proposers, followers, invitees, attendees, and
transcript readers as separate relationships. Three mechanics remain unknown.

## Goal

Identify the concrete low-regret contracts for SIU identity provisioning,
follow-derived event participation, and public-safe Harmonica publication
without implementing an unsettled Atmospheric Groups lexicon.

## Questions

| # | Question |
|---|---|
| D1-Q1 | **Resolved for the bridge:** create or use an organizer-controlled SIU Bluesky account and use its DID as the event identity. The DID/PDS can migrate independently of Community Admin; organizers retain account recovery and custody. |
| D1-Q2 | **Resolved for the bridge:** do not register or claim `#concierge` yet. Community Admin stores a binding to the SIU DID and holds the SIU account app password under its existing per-community service-account pattern (`server/src/service-atproto.js`); organizers retain account custody and rotation. Concierge discovery/delegation remains a standards follow-up after scoped, revocable delegation stabilizes. |
| D3-Q1 | **Resolved:** use unauthenticated `app.bsky.graph.getRelationships` with actor = participant DID and others = SIU DID. `following` proves the relationship; `blocking` / `blockedBy` and list-block fields provide block state. The endpoint accepts up to 30 target DIDs, so it supports point checks without follower-list pagination. Respect AppView rate-limit headers and 429 backoff; cache a positive or negative decision for a bounded TTL. |
| D3-Q2 | **Resolved:** evaluate continuously through the TTL grant. Unfollow retains historical support/availability records but they stop qualifying. Either-direction block and organizer exclusion deny rights. Store exclusion as a tombstone that survives re-follow until an organizer clears it. |
| D3-Q3 | Where is the event-participant grant stored during the bridge phase, and what signed portable record could represent it later without calling it community membership? |
| D3-Q4 | How do Community Admin, Bluesky support ingestion, and Avails consume the same grant without each maintaining a divergent roster? |
| D6-Q1 | **Resolved for the bridge:** the proposal post is the public envelope. It is authored by the SIU DID (via CA on approval), links to the canonical public agenda/topic, and its AT URI/CID is written into the topic file and stored by Community Admin with canonical topic ID, state cache, and timestamps as coordination metadata. |
| D6-Q2 | **Resolved for the bridge:** Harmonica's approval flow calls CA's narrow publish endpoint; CA alone holds the SIU app password and writes the post, returning the AT URI/CID. Scoped Concierge or signed-request publication remains the AV3 target. |
| D6-Q3 | **Resolved for the bridge:** `unconference-brain/` remains authoritative. Community Admin refreshes its projection; retirement or merge stops the post from qualifying and an organizer/CA outcome reply points to the canonical successor, scheduled session, or results. Do not edit historical posts into a second lifecycle ledger. |
| D6-Q4 | **Resolved for the bridge:** use ordinary Bluesky posts for proposals and evaluate `community.lexicon.calendar.event` only when a topic is scheduled. Keep an internal protocol-neutral reference adapter so future group/session lexicons replace the writer without changing canonical topic IDs or URLs. |

## Acceptance

The spike is complete when we can describe:

- the exact SIU DID provisioning, custody, migration, and Concierge-revocation flow;
- the follow-to-event-participant verification and lifecycle contract;
- the one authoritative grant consumed by support and availability systems;
- the minimal public-safe publication envelope and its signer/writer; and
- a bridge path that can run before shared lexicons stabilize without blocking
  or forking the live SIU agenda.

## Current Bridge Map

| Seam | Shipped today | SIU pilot target |
|---|---|---|
| Harmonica binding | Workspace JSON binds `communityId` + `apiBase` to Community Admin; membership checks call bearer-gated `/api/memberships` and cache decisions for five minutes. | Add a provider-neutral event binding keyed by SIU DID without breaking existing local-ID bindings. |
| Community identity | Community Admin keys a Bluesky service account by local community ID; its app password grants whole-repo write access. | SIU DID is the organizer-controlled Bluesky account, stored in the binding. CA holds its app password under the same per-community pattern; scoped delegation replaces this in AV3. |
| Proposal publication | Community Admin call-proposals accept a manually supplied `post_uri` and can auto-publish from per-community app-password accounts. | Harmonica calls CA's narrow publish endpoint on approval; CA posts from the SIU account, returns the AT URI/CID, and stores it against the canonical SIU topic ID. |
| Support | Community Admin already refreshes Bluesky liker DIDs on a TTL. Existing proposal membership gates are standing-community membership. | Count a liker only while their follower-derived SIU event-participant grant is active. Preserve excluded observations without counting them. |
| Availability | Community Admin can send explicit voter DIDs to Avails scheduling; standing availability remains participant-controlled. | Avails accepts the same active event-participant grant/scope rather than enumerating a standing community roster. |
| Harmonica publication | Community Admin's shipped `publish_session` is an agent-mediated event link; Harmonica's SIU V1/V2 does not publish ATProto records. | Harmonica keeps the proposal front door open and links to the SIU profile. Public ATProto writes stay organizer-controlled in the bridge. |

## Evidence

- Harmonica: `src/lib/schema.ts`, `src/lib/community-gate/index.ts`.
- Community Admin: `server/src/memberships.js`, `server/src/service-atproto.js`,
  `server/src/proposals.js`, `server/src/avails-client.js`.
- Bluesky lexicons: unauthenticated `app.bsky.graph.getRelationships` returns
  `following`, `followedBy`, and both-direction block state for up to 30 target
  DIDs; `app.bsky.graph.getFollowers` is paginated and unnecessary for the
  participant point-check path.
