# Meeting notes — product sync 20 Oct

Attendees: 6 people. Product lead, two engineers (including me), design, QA, customer success rep.

## Topics covered

**Feature request backlog triage**  
Customer success surfaced three recurring themes from support tickets: (1) bulk export, (2) better date filtering in the list view, (3) mobile responsiveness on the dashboard. All three have been on the backlog for months. Decision: date filtering moves to the next sprint as a quick win. Bulk export scoped properly for Q1. Mobile responsiveness deferred pending a proper mobile audit first.

**Sprint velocity conversation**  
Last three sprints came in under estimate. Root cause identified as underestimating cross-team dependencies, not coding time. Going to add explicit "dependency resolution" tasks to sprint tickets.

**API versioning**  
Quick discussion on v2 launch timeline. No firm date. Engineering needs two more weeks of internal testing. Customer-facing docs still being written. [Stripe's API versioning approach](https://stripe.com/blog/api-versioning) came up as a reference model — version by date, maintain changelog prominently.

## Action items

- Me: spike on date filtering implementation by end of week
- Design: wireframes for bulk export by Thursday
- QA: draft mobile audit checklist
- Customer success: compile top 10 support ticket themes in a shareable doc

## Notes

The conversation about mobile was revealing — nobody in the room uses the product on mobile regularly so there's genuine uncertainty about what "responsive" even needs to mean in practice. A user research session might be more valuable than an audit first.
