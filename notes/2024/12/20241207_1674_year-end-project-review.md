---
title: Year-end project review — 2024
slug: year-end-project-review
tags: [review, work, annual, projects]
public: true
---

# Year-end project review — 2024

Looking back at projects and technical work from the past year. Not an exhaustive accounting — more a pattern-spotting exercise.

## What went well

**The database migration** came in on time and with fewer issues than anticipated. The key was the dry-run process — running the migration on production data in a read-only mode three times before the actual switch. Found three edge cases that would have caused problems.

**Switching to async standups** [[20241107_1644]] has been a genuine quality-of-life improvement and I'm glad we ran the experiment. The documentation trail is also a bonus — you have a searchable record of what people were working on.

**The fermentation projects** [[20241023_1629]], [[20241108_1645]] — unrelated to work but they've been a useful counterbalance to the screen-heavy parts of the job. Doing something physical and slow with clear feedback loops is genuinely restorative.

## What was hard

The API v2 work took longer than expected, and the delay was mostly communication rather than technical. The requirement changes mid-sprint three times. Will add requirement-lock periods to the process in 2025.

The difficult colleague conversation [[20241113_1650]] didn't land as intended and needed a follow-up. Lesson: these conversations need a quiet room, explicit framing, and more time than I usually allow.

## Technical things I actually learned

- PostgreSQL window functions in depth (see [[20241017_1623]])
- Enough Rust to understand borrowing and why it matters
- Nix shell for development environments — now using it on three projects

## 2025 intentions

- Go deeper on distributed systems rather than wider on frameworks
- Write more — notes, documentation, eventually something public
- Plan better at the start of quarters rather than scrambling at the end
