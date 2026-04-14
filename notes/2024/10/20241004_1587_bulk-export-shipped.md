---
title: Bulk export shipped
slug: bulk-export-shipped
tags: [work, engineering, shipped]
description: Post-ship notes on the bulk export feature
public: true
---

# Bulk export shipped

Went to production this afternoon. Rollout to 10% of accounts initially — expanding to all accounts Monday if metrics look good.

## What shipped

- CSV and PDF export for invoices (up to 10,000 records)
- CSV export for contacts
- Async job architecture with email delivery and S3 storage
- Job status polling endpoint
- 7-day download link expiry

## Numbers from the first few hours

- 47 export jobs initiated
- Average job completion time: 23 seconds (target was <60)
- Zero failures
- Email delivery rate: 100% (this will degrade slightly at scale, normal)

## What I'd do differently

The email template design took more back-and-forth than expected. Getting a designer involved earlier would have saved two iteration cycles. I treated it as an implementation detail; it's actually a UX surface.

The S3 bucket permission model was also fiddlier than anticipated — IAM is always IAM.

## What worked well

The incremental PR approach — merging background job infrastructure, then the CSV exporter, then PDF, then email — meant we got early feedback on the job architecture before building on top of it. Found one issue with the Sidekiq queue configuration that would have been harder to debug with everything in one PR.

## Next steps

Watch for edge cases in the first full week. Known potential issue: customers with non-ASCII characters in invoice line items — tested but not at high volume.

Related design notes: [[20240920_1579]]

[Sidekiq documentation on queue management](https://github.com/sidekiq/sidekiq/wiki/Advanced-Options)
