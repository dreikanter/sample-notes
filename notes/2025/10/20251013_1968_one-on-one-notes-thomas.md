# 1:1 notes — Thomas, October 13

Weekly check-in with my manager. 30 minutes.

## His updates

- The hiring budget is confirmed for two additional engineers; we need to move fast before year-end freeze kicks in
- The notification preferences feature is on the exec dashboard now, which means more scrutiny; he asked for a written risk register
- He's going on a two-week trip in November; will be partially reachable; I'll need to handle escalations

## My updates

- Lena's auth PR merged; blocker resolved
- Deployment reliability work started (runbook updated for the September incidents)
- Interview process: first posting goes live this week

## Discussion points

We spent most of the time on the risk register request. The exec team is nervous about the API changes affecting mobile clients. Agreed to:
1. Draft a compatibility matrix showing what changes are additive vs. breaking
2. Add a feature flag to roll out to 5% of users first
3. Build in a one-week rollback window before full release

He mentioned the engineering blog hasn't had a post in four months and asked if anyone on the team would write something technical. I said I might—the deployment reliability work is a reasonable topic.

## Parking lot for next week

- November coverage plan while Thomas is out
- Q4 OKR alignment — are our team goals still matching company priorities?

## Action items

- [ ] Draft compatibility matrix by Oct 17
- [ ] Implement feature flag in the notification API
- [ ] Look at [engineering blog templates](https://github.com/readme/guides) before deciding whether to write the post
