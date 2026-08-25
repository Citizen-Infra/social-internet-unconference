# Kaliya Young Advisor Input: Open Space And Portable Groups

**Date:** 2026-08-13
**Status:** Design input recorded; launch corrections identified

## Sources

- Direct messages from Kaliya Young shared by Artem Zhiganov on 2026-08-13.
- [Project Weave](https://projectweave.tech/)
- [Project Weave technical depth](https://projectweave.tech/technical-depth.html)
- Kaliya Young, [Free Our Groups: From Platforms to Protocols](https://identitywoman.net/free-our-groups-from-platforms-to-protocols/)
- Kaliya Young, [How Open Protocols Are a Key Part of Regenerative Technology](https://identitywoman.net/how-open-protocols-are-a-key-part-of-regenerative-technology/)

The articles and Project Weave pages were read on 2026-08-13. Claims about
protocol maturity or effects are attributed to those sources rather than
treated as independently verified facts.

## Direct Advice

Kaliya offered three pieces of concrete advice:

1. QiQo Chat has supported IIW and other online-only unconferences. She can
   demonstrate layouts and operating patterns used in practice.
2. Good Open Space begins with a **calling question or theme** and an invitation
   to the people who should be in the room. Framing the event as conference
   **tracks** creates the wrong expectations and should be avoided.
3. Project Weave is exploring groups that use several tools while existing in a
   protocol substrate that belongs to none of them.

## Novel Patterns That Change Or Sharpen SIU

### 1. Calling question and invitation replace tracks

SIU should not choose several top-down tracks. It should settle one compelling
calling question, write an invitation that names the relevant people and
stakes, and let participants propose the sessions. Topic labels may aid
navigation but must not imply fixed program lanes.

**Changes:** README terminology and issue #3. This is a launch concern.

### 2. Portability is a behavioral exit test

Using several applications is not proof of portability. The test is whether SIU
can replace one application while retaining event identity, relationships,
agenda, approved public records, and collective knowledge.

**Changes:** Atmospheric Groups requirements and issue #17. This remains
non-blocking for launch but must pass before SIU claims interoperability.

### 3. A group is more than an event DID

Project Weave describes a group as a first-class object with identity,
membership, governance, trust signals, and history. SIU's Bluesky DID plus
follow-derived event-participant relationship tests a useful subset. It is not
yet a complete portable group object or Verifiable Trust Community.

**Changes:** qualify README and Atmospheric Groups claims; retain AV3 as the
standards-dependent step rather than expanding AV1.

### 4. Functional plurality, not a winning architecture

Project Weave treats interoperability as non-negotiable while allowing
centralized, federated, and peer-to-peer implementations. SIU should preserve
bounded, replaceable roles for Harmonica, Community Admin, Bluesky, Avails,
meeting tools, and Git rather than turn the integration into a new platform.

**Changes:** no new launch task; use the exit test to enforce the existing
"stack, not a venue" decision.

### 5. Evaluate intention, implementation, and consequence

Kaliya's regenerative-technology article maps technology decisions across:

- intentions and values;
- technical design and application; and
- effects on systems, relationships, power, privacy, and dependency.

Each SIU integration decision should therefore record not only what it does,
but who controls the substrate, what happens at end of life, and whether it
builds community capacity or dependency.

**Changes:** add these questions to the Atmospheric Groups evaluation.

### 6. Formation and relationships precede governance machinery

Project Weave frames infrastructure as connective tissue that helps community
form, with governance capacity emerging downstream. SIU should measure useful
relationships and continued coordination, not only attendance, proposals, or
session quality.

**Changes:** strengthens issue #9; it does not replace explicit organizer
governance or safety controls.

### 7. Test information flow without creating another firehose

The source material contrasts useful highlights and bidirectional learning with
high-volume group-chat overwhelm. SIU's durable topic cards, approved session
syntheses, and reviewed links are preferable to making an always-on chat the
canonical record.

**Changes:** confirms `unconference-brain/` and the V7 synthesis/digest path.

### 8. Online Open Space should use tested prior art

A Kaliya walkthrough of QiQo's IIW and online-only unconference layouts can
answer concrete questions about agenda creation, room navigation, participant
orientation, and visible social presence. QiQo is comparative prior art, not a
pre-selected SIU platform.

**Changes:** research step in issue #6 before locking the synchronous layout.

## Later Experiments, Not Launch Scope

- Opt-in, privacy-preserving discovery of participants from the same locality,
  inspired by Kaliya's local/non-local flows.
- A shared behavioral test harness rather than a certification regime.
- Context-specific portable credentials that do not expose unrelated roles.
- A second implementation consuming the SIU public identity, relationships, or
  brain without a Harmonica-private contract.

## Claims To Avoid

- Do not say that the current event DID makes SIU a complete protocol-native
  group.
- Do not say that using multiple tools proves portability.
- Do not repeat "protocols cannot enshittify" as established fact. Open
  protocols can still be captured through dominant implementations,
  infrastructure concentration, governance, or practically unusable exit.
- Do not claim that every Project Weave trust-layer component has the same
  production maturity without verifying each implementation.

## Resulting Actions

- Rewrite the outdated public README around the current event and delivery
  state.
- Reframe issue #3 from tracks to a calling question and invitation.
- Add a QiQo walkthrough/comparative-layout step to issue #6.
- Add the behavioral exit test and three-layer review to issue #17.
- Keep the interoperability proof non-blocking for agenda distribution.
