---
shaping: true
---

# Spike: Jitsi Scheduling, Identity, Attendance, and Recording

## Context

The selected Unconference shape asks Avails to create a Jitsi room when an
organizer promotes a ready topic. CA then needs trusted attendance data for
private transcript access, and the SIU recording connector needs a supported way
to capture the room. The repository README currently proposes MiroTalk SFU for
event rooms, so the relationship between that proposal and Jitsi is not settled.

## Goal

Identify the concrete conferencing, identity, attendance, and recording steps
needed to take one promoted CA topic from an Avails booking to a recorded room
with a trustworthy participant list.

## Questions

| # | Question |
|---|---|
| J1-Q1 | Does Jitsi replace MiroTalk for unconference sessions, or is Jitsi limited to preparation/follow-up calls while the main event remains on another room stack? |
| J1-Q2 | Which Jitsi deployment will SIU use: meet.jit.si, a hosted tenant, or a self-hosted SIU instance? |
| J1-Q3 | How should Avails request a Jitsi room without making Jitsi the default for unrelated bookings? |
| J1-Q4 | How is a high-entropy room URL generated, persisted in the Avails idempotency ledger, included in ICS, and returned to CA? |
| J1-Q5 | How do CA-authenticated identities receive short-lived room credentials, and which stable account claim reaches Jitsi? |
| J1-Q6 | Which signed Jitsi events expose join/leave attendance, and how are retries and duplicate events handled? |
| J1-Q7 | Which recording mechanism is available on the chosen deployment: Jibri, local recording, livestream capture, or a bot/worker? |
| J1-Q8 | How is recording notice and consent represented before a participant joins? |
| J1-Q9 | What media artifact and metadata does the recorder provide to the Meetily adapter? |
| J1-Q10 | What update/cancellation behavior is required across Avails, CA events, Jitsi, ICS, Google Calendar, and recording jobs? |

## Acceptance

The spike is complete when we can describe the exact API calls, credentials,
claims, event payloads, and failure/retry behavior for booking one room,
authenticating attendees, recording it with consent, and handing the media to
the Meetily adapter. It must also state whether this changes the MiroTalk room
proposal in the repository README.
