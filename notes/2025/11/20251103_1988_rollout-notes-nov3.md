# Notification preferences rollout — November 3

Started the 5% rollout for notification preferences v2 today at 2 p.m.

## Launch steps

1. Updated flag in Redis: `notification_v2` → `enabled: true, percentage: 5`
2. Verified via health endpoint that flag was being read correctly
3. Confirmed in logs that approximately 5% of incoming requests were routing to v2 handler
4. Set up dashboard alert for error rate increase on v2 path

## First hour

Error rate on v2 is 0.2%, which is within normal range (v1 baseline is 0.15%). The small difference may just be the novelty of new code paths.

Three specific users reported via support that their notification settings weren't saving. Investigated:
- Two were on v1 (false positive — their issue predated the rollout, unrelated)
- One was genuinely on v2 and experiencing an issue: the preference for "digest" mode wasn't persisting because the enum in the DB wasn't migrated yet

Found the migration bug. We had the migration written but it hadn't run on prod yet. Applied it at 3:45 p.m. Issue resolved.

## Post-launch

Error rate normalized to 0.18% by end of day. No other issues.

## Next steps

- Monitor for 48 hours at 5%
- If stable, bump to 25% on Nov 5
- Full release (100%) target: Nov 12

See the compatibility matrix from [[20251013_1968]] for the full risk register.
