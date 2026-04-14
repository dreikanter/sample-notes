# GraphQL vs REST — working notes

After using both in production, some hard-won opinions.

## Where GraphQL wins

**Flexible data fetching:** Clients specify exactly what they need. Eliminates over-fetching (getting too much) and under-fetching (needing multiple round trips). Essential for mobile clients where bandwidth matters.

**Aggregation layer:** When you need to present a unified API over multiple services or data sources, GraphQL's resolver model is more natural than orchestrating REST calls.

**Rapid frontend development:** Frontend teams can move without waiting for backend to add a new endpoint. They can compose queries from existing types.

**Introspection and tooling:** The schema is self-documenting. Tools like GraphQL Playground and the type system provide instant feedback.

## Where REST wins

**Caching:** REST maps naturally to HTTP caching (URLs are cache keys, Cache-Control headers work as expected). GraphQL POST requests to a single endpoint are harder to cache at the HTTP layer.

**Simplicity:** REST is conceptually simpler. Any HTTP client works. No special tooling required. The learning curve for contributors is lower.

**File uploads:** Awkward in GraphQL. Easy in REST with multipart form data.

**Streaming / webhooks:** Server-sent events and webhooks are simpler with REST.

## Current opinion

REST is the right default. GraphQL is worth the complexity when you have: multiple client types with different data needs, or an aggregation layer over many services, or a product where frontend teams frequently need data combinations that don't map to existing endpoints.

Adding GraphQL to REST isn't an either-or — using GraphQL for the dynamic client-facing API and REST for internal service-to-service is a reasonable hybrid.

https://graphql.org/learn/thinking-in-graphs/
