# Social Internet Unconference

The Social Internet Unconference (SIU) is an online Open Space event for people
building a more humane, interoperable social internet: open protocols,
community governance, deliberation, sustainable funding, and the relationships
that let this work continue.

The event is being developed by CIBC with collaborators from adjacent
communities. It is not owned by any one participating organization or software
application.

## Current Status

As of 2026-08-13:

- The public agenda is live at
  [app.harmonica.chat/p/social-internet-unconference](https://app.harmonica.chat/p/social-internet-unconference).
- Anyone can use the
  [topic-proposal conversation](https://app.harmonica.chat/chat?s=62e14a98-d554-401d-9566-616c30cc6e83)
  without joining a community or signing in with Bluesky.
- The Unconference project type, public record, and bidirectional
  `unconference-brain/` round-trip are deployed (V1).
- Facilitated multi-topic proposal formation and review are deployed and the
  complete two-topic lifecycle is validated (V2).
- Bluesky publication and follower-qualified proposal support are planned next
  (V3 / Atmospheric Groups AV1). V4-V8 have not started.
- The event date, scale, synchronous lobby, main-event room stack, and
  distribution channel remain open.

The cross-repository delivery state is tracked in [issue #16](https://github.com/Citizen-Infra/social-internet-unconference/issues/16).

## Event Shape

SIU uses Open Space Technology rather than a keynote-and-tracks conference
model. Participants help form the agenda and can move between sessions.

The current design direction, reinforced by advisor Kaliya Young, is to:

- develop one compelling **calling question or theme**;
- write an invitation that makes clear who should be in the room and why;
- let session topics emerge from participants; and
- avoid calling topic groupings **tracks**, which creates conventional
  conference expectations.

The calling question and invitation are not yet settled. See
[issue #3](https://github.com/Citizen-Infra/social-internet-unconference/issues/3)
and [issue #5](https://github.com/Citizen-Infra/social-internet-unconference/issues/5).

## Participation

- **Browse:** the public agenda is open.
- **Propose:** anyone can complete an AI-facilitated topic conversation, with a
  maximum of three topics per conversation.
- **Support (planned):** follow the SIU Bluesky profile and like a proposal
  post. Only likes from DIDs with an active event-participant relationship will
  count; unfollows, blocks, and organizer exclusions revoke qualification.
- **Availability (planned):** event participants will be able to publish
  reusable SIU-scoped availability through Avails.
- **Invitation, attendance, and transcript access:** these are separate,
  explicitly granted relationships. Following SIU does not create standing
  community membership or organizer authority.

## A Stack, Not A Platform

No single application owns the unconference:

- **`unconference-brain/` in this repository** is canonical for public-safe
  topic and session meaning.
- **Harmonica** facilitates proposals, review, public agenda display, and later
  reflection; it is one contributor to the record, not its owner.
- **Community Admin** coordinates the SIU event identity, Bluesky publication,
  support qualification, scheduling, and bounded application permissions.
- **Bluesky / AT Protocol** provides the event's public identity and the planned
  follow-and-like participation surface.
- **Avails** will hold participant-controlled standing availability and select
  compatible times.
- **Jitsi/JaaS, Excalidraw, Google Calendar, My Community, and SIU-controlled
  artifact infrastructure** have bounded session and distribution roles in
  later slices.
- **Streamplace** remains a candidate public broadcast/discovery layer. It is
  not the room, lobby, attendance source, or private artifact store.

The selected identity pilot uses an organizer-controlled SIU Bluesky DID with
event-scoped relationships. This is a practical bridge toward a portable group
object, not a claim that SIU already implements a complete Verifiable Trust
Community or platform-independent group protocol.

## Portability Test

Project Weave's work on groups as first-class protocol objects sharpens SIU's
interoperability test. Connecting several APIs is not enough. The behavioral
question is:

> Can SIU replace an application while retaining its identity, relationships,
> agenda, approved public record, and collective knowledge?

The launch bridge does not yet prove that. The Atmospheric Groups pilot tracks
a non-blocking, post-launch exit test and an independent consumer of the public
group record in [issue #17](https://github.com/Citizen-Infra/social-internet-unconference/issues/17).

## Repository Map

- [`unconference-brain/`](unconference-brain/) - canonical public-safe topics
  and, later, session records.
- [`docs/plans/unconference-platform-shaping.md`](docs/plans/unconference-platform-shaping.md) - selected cross-system shape and decisions.
- [`docs/plans/unconference-platform-slices.md`](docs/plans/unconference-platform-slices.md) - V1-V8 implementation sequence.
- [`docs/plans/atmospheric-groups-pilot-shaping.md`](docs/plans/atmospheric-groups-pilot-shaping.md) - event-DID and scoped-relationship shape.
- [`docs/plans/atmospheric-groups-pilot-slices.md`](docs/plans/atmospheric-groups-pilot-slices.md) - AV1-AV3 identity slices.
- [`docs/research/2026-08-13-kaliya-young-advisor-input.md`](docs/research/2026-08-13-kaliya-young-advisor-input.md) - advisor input, Project Weave comparison, and resulting design implications.
- [`docs/research/2026-08-13-qiqo-walkthrough-brief.md`](docs/research/2026-08-13-qiqo-walkthrough-brief.md) - decision-focused guide for Kaliya's online Open Space walkthrough.

## Open Decisions

- Calling question and invitation: [#3](https://github.com/Citizen-Infra/social-internet-unconference/issues/3), [#5](https://github.com/Citizen-Infra/social-internet-unconference/issues/5)
- Date, duration, scale, and online Open Space layout: [#6](https://github.com/Citizen-Infra/social-internet-unconference/issues/6)
- Co-organizing capacity: [#7](https://github.com/Citizen-Infra/social-internet-unconference/issues/7)
- Post-event continuity: [#9](https://github.com/Citizen-Infra/social-internet-unconference/issues/9)
- Main-event broadcast path: [#2](https://github.com/Citizen-Infra/social-internet-unconference/issues/2)
- Atmospheric Groups portability pilot: [#17](https://github.com/Citizen-Infra/social-internet-unconference/issues/17)

## Privacy Boundary

This public repository contains only public-safe coordination data. Do not
commit voter identities, availability details, attendee lists, private proposal
responses, full transcripts, stream keys, app passwords, or bearer tokens.
