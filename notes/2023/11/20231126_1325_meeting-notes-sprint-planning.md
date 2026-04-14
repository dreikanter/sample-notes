# Meeting notes — sprint planning, November 26

**Attendees:** Priya, Marcus, Devlin, me  
**Duration:** 1h 10min (ran over)

## Capacity

Devlin is out Wednesday–Friday for the holiday. Marcus has 60% availability this sprint due to the on-call rotation. Realistic capacity: roughly 18 story points rather than our nominal 24.

## What we took in

1. Rate limiter config cleanup (3 pts) — I'm owning this, carry-over from last sprint
2. Reporting table indexes (2 pts) — already done, needs PR review from Priya
3. Auth token refresh edge case (5 pts) — Marcus taking lead, I'll review
4. Background job retry logic (8 pts) — Devlin starting Monday, needs design review before writing code

Deliberate decision: left the notification preferences UI out of this sprint. It kept getting pointed higher in estimation because nobody has dug into the frontend state management yet. We scheduled a thirty-minute design session for Thursday.

## Issues raised

Marcus raised the alert fatigue problem again — we're generating too many low-severity Slack pings and people are tuning them out. Agreed to audit the alert configs next sprint. Priya volunteered to categorize the past month's alerts by actionability.

The retro action item from two sprints ago (documentation for the event pipeline) has not moved. Assigned it formally to me with a deadline. It will be done before December 15.

## Action items

- [ ] Merge index PR after Priya reviews — me, by Tuesday
- [ ] Design session for notification preferences — Thursday 2pm
- [ ] Alert audit plan — Priya, by end of sprint
- [ ] Event pipeline docs — me, by Dec 15

Reference for our sprint process: [Shape Up by Basecamp](https://basecamp.com/shapeup) influenced how we structure design sessions, even though we use Jira for tracking.
