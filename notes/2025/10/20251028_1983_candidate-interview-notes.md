# Interview notes — backend candidate, October 28

Candidate: referred to internally as C-1 (two candidates this week).

## Overall impression

Strong. Five years of backend experience, three in distributed systems contexts. Came prepared—had looked at our engineering blog (such as it is) and read the recent incident post-mortem we published.

## Technical assessment

**Database question:** Gave a thorough answer. Named EXPLAIN ANALYZE unprompted, discussed index selectivity, mentioned the difference between Seq Scan and Index Scan accurately. When I pushed on the "when NOT to index" question, gave a good answer about write amplification and maintenance cost on high-write tables.

**Distributed systems question:** Very good. Described circuit breaker pattern correctly, mentioned Hystrix and then said "but we don't actually use it—we implemented a simpler version ourselves," which is the kind of honest engineering answer I like.

**Debugging scenario:** Less strong. Gave a methodical approach (check logs, check metrics, check memory profiler) but didn't get to the idea of heap dumps or tracking object retention across GC cycles unprompted. When I led there with a follow-up question, picked it up quickly.

## Collaboration question

Good answer. Described a disagreement about database schema design—their team wanted a normalized schema, the DBA wanted a denormalized one for read performance. They built a prototype to settle the argument. That's exactly the right instinct.

## Their questions

Asked about how we handle on-call, which teams own what, how postmortems work. Good questions—focused on process and learning culture.

## Recommendation

Strong yes to move forward. Schedule the technical screen with the full team.

Reference: interview rubric from [[20251008_1963]]
