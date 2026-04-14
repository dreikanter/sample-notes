---
title: Search redesign shipped
slug: search-redesign-shipped
tags: [work, shipping, frontend]
---

# Search redesign shipped

Deployed to production this afternoon after a week of testing and two rounds of QA feedback.

**What shipped**

The new search UI with:
- Redesigned input with clearer active/inactive states
- Bottom sheet filter panel on mobile (Option A from the May design review, [[20230522_1164]])
- Keyboard navigation per the spec ([[20230522_1164]])
- Improved empty state and error state designs
- Analytics events for the new flow

**Metrics from the first two hours**

- Search usage up ~8% (likely a novelty effect, will monitor over a week)
- No errors in the error tracking dashboard
- LCP on the search page: 2.1 s (was 3.4 s) — significant improvement from the component refactor

**What was cut for this ship**

- The "save search" feature (deferred to Q3)
- The bulk action on results (complexity, moved to separate epic)

**Post-ship**

One user report within 90 minutes: the filter panel on certain Android devices (Samsung Galaxy with an older Chrome version) wasn't opening on first tap. Reproduced, traced to a passive event listener issue. Deployed a fix within an hour.

**Lessons**

The hook architecture (from [[20230620_1188]]) made the testing fast — each piece was testable in isolation. The URL sync was the last piece and the cleanest because everything else was already sorted.

[Web.dev case study format](https://web.dev/case-studies/)
