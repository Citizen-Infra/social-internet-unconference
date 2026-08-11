# Social Internet Unconference

An online unconference at the intersection of the social internet, governance, and how this kind of work sustains itself — developed by CIBC with Erlend Sogge Heggen of [Roomy](https://roomy.space).

This repo is the **planning home** while the event is still being shaped. It is not the event site. Nothing here is announced — much of it is still undecided, and the open questions below are the actual work.

## Status

**As of 2026-07-30** (presented and discussed at the CIBC town hall — [recording](https://fathom.video/calls/765095900), roughly 21:30–51:06 of it):

**Settled or moved forward**

- One advisor confirmed: **Tracy Kunkler** on Open Space Technology as the format.
- The **Roomy coordination space exists** — a channel in Erlend's "good tech" space, though not yet prepped.
- **Six candidate themes** on the table, up from one locked theme.
- A **structural frame** for the work: two work streams, not two projects (below).
- **Invite criteria** stated, and a first concrete lead.

**Then, the same evening**, Erlend proposed a concrete stack and a funding route in a message to Artem (18:52), and Artem named the format problem as a service-design challenge (19:08). Both are folded in below.

**Still open**

- **No name.** No date. No decision on scale.
- No invite list, and no co-organiser for the organising work stream.
- **Nobody has contacted Eli at Streamplace** — unchanged since 2026-07-21, though it is no longer a blocker (see Infrastructure).

Artem's assessment on the call: *"it is a complex project — it's the most complex project that we have ever discussed so far in CIBC."*

This repo is private because it names people who have not been approached and quotes private conversations. Before it is made public, the README and every issue need scrubbing for both.

## What this is

An unconference rather than a keynote conference — participants agree the topics together at the start and move between rooms, as in the in-person events the idea came from. How that works when everyone is remote is the open question (issue #6). Tracy Kunkler is advising on **Open Space Technology**, the methodology behind that format.

The framing is deliberately cross-community: not a CIBC event and not a Roomy event, but a shared one pulling several adjacent communities around a common set of questions.

**Note on scope.** This began as an unconference about *funding the social internet*. As of the 2026-07-30 discussion, funding is one thread among several rather than the subject — see Themes.

## Two work streams, one project

Raised by Chris Bland on the town hall: two different things were being discussed as if they were one.

1. **Design** — how an ideal unconference should run, and what platforms and tech could support it. Chris's framing: *"the most human-centered, frictionless, decentralized, unowned, non-data-extractive kind of way."* This stream now has concrete questions in it: identity for MiroTalk, a composed Streamplace broadcast path, and the lobby below.
2. **Organisation** — themes, tracks, people, invitations, logistics.

Artem accepted the distinction but not a split into separate projects: *"it's not two different projects, it's just two different work streams."*

**The lobby.** The hardest design problem, named by Artem (2026-07-30 19:08): *"I'm trying to think of it as a service design challenge. How can people co-create the schedule in the morning? I guess we need some kind of lobby for that."* An unconference agrees its agenda in the room; there is no obvious remote equivalent, and if rooms are MiroTalk instances then the lobby is whatever surface exists *before* rooms do. Note this is **synchronous, morning-of** — distinct from #8, which proposes Harmonica for **async, pre-event** agenda forming. The event may want both; nobody has decided.

**Working method**, proposed by Megan Ducote and adopted as the sensible shape given capacity: small working groups that go away to research and collaborate, then reconvene — rather than everything running through one coordinating person.

## Who does what

- **Artem Zhiganov (CIBC)** — drives and coordinates.
- **Erlend Sogge Heggen (Roomy)** — shaping and theming, and attends. He was explicit that coordination is not his role.
- **Tracy Kunkler** — advising on Open Space Technology as the format. Confirmed 2026-07-30. Her capacity is limited and she said so directly; scope asks to something specific and small rather than open-ended organising.
- **Megan Ducote (CIBC)** — proposed the small-working-groups method and the after-event constraint (#9). Role not defined beyond that.

**Repo access:** three admins — Artem (`zhiganov`), Erlend (`erlend-sh`), Megan (`HoustonBloom`). Anything written here is visible to all three; it is not a staging area.

**Still no co-organiser for the organisation work stream** (#7) — repo access is not the same as having taken that on, and nobody has.

## Themes

Six candidates as of 2026-07-30. **None are locked**, and which are central versus breakout tracks is undecided — see issue #3.

- **AT Protocol** and **the social internet** — the spine.
- **Deliberation** — Nicolas Gimenez and Yuting Jiang (Agora Citizen Network) are building a large-scale online deliberation tool.
- **DDS** — the W3C Decentralized Deliberative Stack. Floated as a track of several sessions rather than a single slot.
- **The humane thread** — governance from a less technical, more human perspective. Artem argued for it specifically: *"there is something else besides technology and governance that is really important, which is maybe more humane, maybe more spiritual… it shouldn't be all about optimization."* Anchored on Tracy's *Governance That Flows* writing.
- **Funding non-profit and open-source work** — the original theme, now one of several. Stated as a shared and personal problem: *"it's something that many of us are struggling with. I know that I'm struggling with this myself."*

## Who it is for

Criteria stated on 2026-07-30: **mission-aligned, working on online communities, and interested in non-blockchain decentralisation.** Explicitly not the Web3 ethos — Artem: *"Web3 has developed this very particular ethos or maybe culture, which I really want to distance myself from… But decentralization is one of the most important things that all of us should work on."*

The strategic reason for the guest list is partnership, not attendance: the event is meant to *"jumpstart"* a networked cooperative ecosystem of aligned organisations sharing resources.

**First concrete lead: [Internet Identity Workshop](https://internetidentityworkshop.com/)** — raised by Nicolas Gimenez as one of the earliest adopters of the unconference format, from the decentralised identity movement. Both a community to invite and a format precedent to study.

## Infrastructure

**Coordination**

- **Roomy — exists.** A channel inside Erlend's existing "good tech" space rather than a dedicated server. Erlend: *"we just quickly threw something up just to have it done, but it hasn't been prepped really for anything yet."* Seeding it is issue #4.
- **Also a Telegram thread** in the CIBC group — decided on the call to run both. Worth watching: the Season 2 retrospective's loudest operational complaint was that async coordination is already stifled and Telegram hard to sort through, so running two surfaces needs one of them to be clearly primary.

**The event itself — a stack, not a venue**

Erlend's proposal (2026-07-30) replaces "Streamplace hosts the unconference" with a two-layer stack:

- **Rooms: [MiroTalk SFU](https://github.com/miroslavpejic85/mirotalksfu)**, extended with **AT Proto logins**.
- **Streaming out: [Streamplace](https://stream.place)** — hosted Streamplace accepts RTMPS or WHIP, but MiroTalk's documented RTMP support does not compose a conference and publish it to an arbitrary external endpoint. The bridge remains unverified.

**Streamplace is open source and presents a self-hosting track**, its API is AT Proto lexicons (`place.stream.*` XRPC, `did:web:stream.place`), and hosted broadcaster identity uses AT Protocol OAuth. That identity authenticates the public broadcaster; it does not automatically give MiroTalk participants one verified identity across both systems. Contacting Eli can be relational rather than a hosted-service dependency, but the technical path still requires a compositor and a verified deployment.

Caveats, because this is not yet verified end to end: the self-hosting installation URL still redirects to the hosted application when checked (2026-08-11), so nobody has confirmed the production install process; Streamplace's hosted flow is public; and its capacity should not be assumed to absorb whatever scale #6 lands on without a load test. The integration contract and gates are captured in [the Streamplace broadcast spike](docs/plans/spike-streamplace-main-event-broadcast.md).

**Alternative to MiroTalk:** [suitenumerique/meet](https://github.com/suitenumerique/meet), which Erlend rated technically comparable but rejected on funding grounds — *"then the sponsorship source is something more nebulous."*

**Funding**

- **Recall.ai** proposed as a sponsor (#10): they already sponsor MiroTalk. Note the reflexivity — funding open-source work is one of the candidate themes, so how this event is funded is itself on topic.

## A constraint worth designing for

Megan Ducote, on what would make this succeed or fail:

> "the success metrics would be like the connections people make and go off after that… you create the space for people to come together and say, hey, I found someone I'm interested in, they're doing similar work, but **there's nowhere for them to go after**."

Connections that continue are the measure, not attendance or session quality. Issue #9.

## Open questions

Each is an issue in this repo.

1. **#1** — What is it called?
2. **#2** — Which stack: hosted Streamplace, self-hosted, or MiroTalk streaming into it? And separately, do we want Eli involved?
3. **#3** — Which themes are central, and which are tracks?
4. **#4** — Seeding the Roomy space: what channels does it need?
5. **#5** — Who is invited?
6. **#6** — When does it happen, how long, and for how many?
7. **#7** — Who co-organises?
8. **#8** — Using Harmonica to form the agenda and capture session reflections.
9. **#9** — Where do connections go afterwards?
10. **#10** — Sponsorship: approach Recall.ai, and does the stack choice follow the money?

## Background

This came out of a conversation between Artem and Erlend on 2026-07-21 about AT Proto, open-source funding models, and how the independent-developer ecosystem sustains itself. It was presented to the wider CIBC membership at the 2026-07-30 town hall, where it was the most-discussed item.
