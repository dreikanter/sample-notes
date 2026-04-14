# Infrastructure cost audit — initial findings

Started the infrastructure cost review I planned for October. Two hours of looking at bills and tagging. Initial findings are mixed.

## What we're spending

Monthly cloud costs have grown from £3,400 in January to £6,100 in September. Revenue has grown faster than this, so the ratio is improving, but the absolute growth deserves scrutiny.

Main categories:
- Compute (EC2/Lambda): 48%
- Database (RDS): 31%
- Storage and CDN (S3, CloudFront): 12%
- Other (queues, observability, misc): 9%

## Easy wins found

**Oversized RDS instance:** Our primary database is on a db.r6g.2xlarge. Looking at average CPU utilization (CloudWatch metrics, last 90 days): p95 is 22%, p99 is 61%. We're provisioned for bursts that rarely happen. A db.r6g.xlarge would handle current load with a comfortable margin. Potential saving: ~£900/month.

**Unattached EBS volumes:** Found 7 EBS volumes from old instances, not attached to anything. Total: 1.4TB. Snapshotting and deleting. Saving: £140/month.

**Development environment running 24/7:** The staging and development environments run continuously. They're used 9am-7pm most days. Scheduled shutdown could save 50% of their compute cost. Saving estimate: £200/month.

## What needs more investigation

The Lambda costs spiked in August. The bulk export feature is async but the job processing seems more expensive than expected. Might be worth profiling.

[AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/) has the right granularity for this analysis.
