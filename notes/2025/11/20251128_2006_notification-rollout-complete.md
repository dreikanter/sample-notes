---
title: Notification preferences — rollout complete
slug: notification-rollout-complete
tags: [work, engineering, deployment]
---

# Notification preferences — rollout complete

The notification preferences v2 went to 100% of users today. Timeline:

- Nov 3: 5% rollout, bug found and fixed (see [[20251103_1988]])
- Nov 5: 25% rollout, clean
- Nov 12: 75% rollout, clean
- Nov 28: 100%, clean

No customer-reported issues since the DB migration bug on day one. Error rate on v2 is 0.13%, below the v1 baseline of 0.15%.

## What went well

Feature flags made this much less stressful than previous full releases. Being able to move in increments and observe at each stage meant we caught the real production issue early and at low impact.

The compatibility matrix (done for Thomas's risk register request) also turned out to be useful for the team—it forced us to enumerate what was actually changing and communicate it to the mobile teams properly. They flagged one potential issue we'd missed; we addressed it before it became a problem.

## Metrics after one week at 100%

- User engagement with notification preferences settings: +34% (people are changing settings more, which is the point)
- Support tickets related to notifications: -18% (fewer unexpected notifications)
- API latency: p99 slightly higher (22ms vs. 18ms) due to additional permission checks—acceptable

## Post-launch

Removing the feature flag next week—once you're at 100% with no rollback plan needed, flags are just dead code.

Write the engineering blog post.

Reference: [Feature flag cleanup checklist from LaunchDarkly](https://launchdarkly.com/blog/feature-flag-debt/)
