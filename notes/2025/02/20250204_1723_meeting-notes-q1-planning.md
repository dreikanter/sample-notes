---
title: Meeting notes — Q1 planning sync
slug: meeting-notes-q1-planning
tags: [work, meetings, planning]
---

# Meeting notes — Q1 planning sync

**Date:** 2025-02-04
**Attendees:** Priya, Marcus, Selin, me
**Duration:** 90 min

## Agenda items covered

### API versioning strategy

We've been deferring this for months. Agreed on a deprecation timeline: v1 endpoints stay live until June 30, v2 ships in March, we send comms to affected API consumers by March 15. Marcus owns the comms draft.

### Infrastructure cost review

Selin walked through the January numbers. Compute costs up 18% month-over-month, mostly due to the new ML inference endpoints we stood up in December. Nothing alarming but worth watching. She's setting up a weekly cost digest in Slack.

### Hiring

Two open reqs: one senior backend, one data engineer. Priya has screened 12 candidates for the backend role, moving 4 to interviews. Data engineer req is newer — job description needs one more pass before posting.

### Q1 goals check-in

- Data pipeline reliability: on track
- Dashboard redesign: delayed by 2 weeks (design reviews took longer than planned)
- API v2: at risk — depends on finishing the auth layer by end of February

## Action items

- Marcus: draft API deprecation comms by Feb 14
- Selin: set up cost digest channel
- Me: finish auth layer spec this week

## Resources

Good framing for deprecation communication: [https://stripe.com/docs/upgrades](https://stripe.com/docs/upgrades)
