# Unconference Brain

This directory is the durable, human-readable record for the Social Internet
Unconference. It lives inside the `social-internet-unconference` repository;
it is not a separate repository.

## Contents

- `topics/` contains one Markdown file per stable topic ID.
- `sessions/` contains one directory per stable session ID.

The directory is safe to mirror into Harmonica. It contains public-safe
coordination data only. Do not commit voter identities, attendee lists, private
conversation evidence, full transcripts, stream keys, or bearer tokens.

Harmonica-generated changes must be deterministic and idempotent. Human edits
are canonical input and must be reviewable when they do not satisfy the file
contract.
