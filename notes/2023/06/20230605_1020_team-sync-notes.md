---
title: Team sync notes — June 5
slug: team-sync-notes
tags: [meetings, work, planning]
---

# Team sync notes — June 5

Attendees: Priya, Tomás, Lena, me.

## Status updates

**API migration**: Tomás finished the auth layer last Friday. Outstanding: rate-limit headers on the new endpoints and one test flap in CI that seems environment-dependent. He's not going to block the merge on the flap but wants it logged. I agreed to open the issue before end of day.

**Design review**: Lena pushed the updated component library to staging. The date-picker behaviour changed — not a regression, an intentional fix for the timezone offset bug (#441), but the QA team wasn't briefed so there will be confusion. Lena is going to send a short note to the QA channel.

**Documentation**: Still mine to finish. The API reference docs are 80% done; the remaining 20% is the error-code table which requires input from Priya on the planned additions. Priya will send her list by Thursday.

## Decisions made

- We're going with pagination over cursor-based scrolling for the search endpoint. Reason: the client-side implementation is simpler and search result sets rarely exceed 500 rows in current usage.
- The deprecation of `/v1/users/legacy` is pushed to Q3, not end of May as originally planned. External partners requested more time.

## Action items

- Me: open issue for CI test flap before EOD
- Me: finish error-code table once Priya sends list
- Lena: send QA note about date-picker change
- Tomás: merge auth layer PR after CI clears

Next sync: June 19.

Reference: [RFC 7807 Problem Details for HTTP APIs](https://www.rfc-editor.org/rfc/rfc7807) — came up in the error-code discussion.
