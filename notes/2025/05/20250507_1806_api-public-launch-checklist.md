---
title: API public launch checklist
slug: api-public-launch-checklist
tags: [work, api, planning]
---

# API public launch checklist

Targeting May 15. Working through this as items complete.

## Must-have before launch

- [x] Auth error codes standardized (expired, revoked, malformed, insufficient-scope)
- [x] HTTP caching fixed for auth-gated endpoints (see [[20250418_1790]])
- [x] Rate limiting confirmed working (tested at 2x expected load)
- [x] Onboarding redesigned to 3 steps
- [x] v1 deprecation comms sent to all existing users
- [ ] Docs reviewed by someone unfamiliar with the codebase (scheduled for May 9)
- [ ] Status page set up (using Statuspage.io)
- [ ] Support channel announced (Slack community + email)
- [ ] Runbook for common incidents written
- [ ] Load test results reviewed and within acceptable parameters

## Nice to have (post-launch okay)

- [ ] Client library for Python published (drafted, needs review)
- [ ] Sandbox environment (lightweight version — separate from production data)
- [ ] Changelog page on the docs site

## Rollout plan

- May 12: internal dry run (simulate launch day)
- May 14 (day before): full team on call, monitoring dashboards open
- May 15 08:00 PT: flip the switch — update the landing page, send the announcement email
- May 15-16: active monitoring, quick response to any issues
- May 19: first post-launch retrospective

## Incident response plan

Primary on-call: me
Secondary: Selin
PagerDuty escalation policy: configured

Reference incident response: [https://response.pagerduty.com](https://response.pagerduty.com)
