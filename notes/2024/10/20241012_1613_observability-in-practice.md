# Observability in practice — what actually helped

After setting up OpenTelemetry [[20240924_1581]] and running it for three weeks, some reflections on what proved useful versus what seemed like it would be useful.

## What proved genuinely valuable

**Distributed traces for the bulk export job.** The async job was hitting a timeout I couldn't explain. The trace showed the slow span immediately — it was an N+1 query in the invoice serialization, 1,400 individual SQL queries for a 1,400-invoice export. Without the trace I'd have been guessing which of a dozen potential slow spots was the problem.

**Error rate dashboards correlated with deploy events.** We now overlay deployment markers on all error rate graphs. Catching regressions introduced by a deploy has gone from "someone eventually notices" to "alert fires within 5 minutes." This is clearly better.

**Latency histograms rather than averages.** Average latency hides tail latency problems. Our p99 for the export email sending was 45 seconds (within the timeout) but the mean was 6 seconds. The histogram revealed that 1% of emails were hitting a retry cycle.

## What didn't move the needle much

**The log aggregation changes.** We restructured logs to be more parseable. It's slightly nicer but hasn't changed how we debug. The traces were the thing.

**Custom metrics for "business" events.** Instrumenting events like "invoice exported" sounded valuable. In practice we query this from the database because we need the richer context. The metrics are redundant.

## The lesson

Traces are the highest-value investment for a services architecture. Logs remain important for specific error detail. Metrics are most useful at the infrastructure level. [Cindy Sridharan's writing on observability](https://copyconstruct.medium.com/) shaped this view.
