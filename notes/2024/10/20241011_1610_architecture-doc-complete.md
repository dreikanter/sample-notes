# Architecture document completed — new API layer

The architecture doc that's been on my list since June [[20240629_1524]] is done. Four months late, but the delay was partially productive — the design has changed three times since June and the final version is better for having waited.

## What it documents

The new API layer is a separate service that sits between our client applications and the existing backend. Rationale:

- Our main backend has accumulated 8 years of design decisions. Some are good; some reflect problems that no longer exist.
- The new API layer lets us build a clean public API without requiring full backend refactoring
- Versioning is easier at the edge than inside the monolith

## Key decisions documented

**Protocol:** GraphQL for internal clients, REST for the public API. This was contentious. The argument for GraphQL internally: our client teams were constantly asking for new query patterns and we were building bespoke endpoints. GraphQL eliminates that conversation. The argument for REST publicly: GraphQL's introspection and query complexity create security considerations that are easier to avoid with REST.

**Authentication:** OAuth 2.0 with JWT tokens. Existing session-based auth continues for the main application.

**Rate limiting:** The layer we already built [[20240822_1564]] integrates here as middleware.

## Why it took so long

The honest answer in the doc's introduction: "The delay reflected genuine uncertainty about the approach. Forcing a decision in July would have produced a worse document and a worse architecture."

Sometimes "this is late" and "this took the right amount of time" are both true.

[Architecture decision records (ADRs)](https://adr.github.io/) — the format I used for each major decision.
