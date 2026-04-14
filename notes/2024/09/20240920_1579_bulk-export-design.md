---
title: Bulk export feature — design notes
slug: bulk-export-design
tags: [work, engineering, design]
description: Design decisions for the bulk export feature
---

# Bulk export feature — design notes

Starting the bulk export sprint. Design decisions captured before implementation begins.

## Scope

Users can export invoices and contacts in CSV and PDF formats. Individual export exists; this is batch (up to 10,000 records per export).

## Key design decisions

### Async job architecture

Synchronous export for 10,000 records will timeout. The answer is obvious: async job with a download link sent by email. Architecture:

1. User requests export → creates a job record → returns 202 Accepted with job ID
2. Background worker processes export → uploads result to S3
3. Email sent with presigned download URL (7-day expiry)
4. Job status endpoint for polling (optional but useful for future UI work)

### Queue choice

Using the existing Sidekiq setup. Export jobs get a dedicated queue with lower concurrency than real-time jobs — exports are CPU-intensive and should not compete with user-facing requests.

### Data snapshot vs live data

Exports should reflect data as of the moment the request was made, not as of when the job runs. For most jobs this is a few seconds difference; for jobs that queue overnight it could be significant.

Solution: the job record stores the full query parameters and the requested-at timestamp. The worker queries with a time bound.

### File format decisions

CSV: straightforward. UTF-8 with BOM for Excel compatibility (Excel has famously inconsistent CSV parsing).

PDF: using Puppeteer (already in the stack from the invoice work). Renders HTML template server-side, then PDF-prints it. This gives us design consistency without a separate PDF library.

Reference: [AWS S3 presigned URLs documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html)
