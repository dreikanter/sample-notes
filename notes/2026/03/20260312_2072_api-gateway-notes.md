# API gateway architecture notes

Notes from the decision meeting. Three proposals considered.

## Proposal A: BFF per client type

Backend-for-frontend pattern. Separate gateway services for mobile clients and web clients, each translating between upstream microservices and the needs of its client.

**Pro**: client teams own their gateway; responses shaped to actual needs.
**Con**: code duplication across BFFs; cross-cutting concerns (auth, rate limiting) implemented twice; harder to maintain as service count grows.

## Proposal B: Single GraphQL gateway

One gateway exposing a GraphQL schema; resolvers call upstream REST services.

**Pro**: clients get exactly what they ask for; one place for auth and rate limiting; schema as contract.
**Con**: GraphQL complexity overhead; N+1 query problem if resolvers aren't DataLoader-aware; steep learning curve for non-GraphQL engineers on the team; some mobile clients doing GraphQL query construction is non-trivial.

## Proposal C: Lightweight proxy with request routing

Thin gateway (we looked at Kong and a custom Go service) handles auth, rate limiting, and routing. Upstream services remain REST. No translation layer.

**Pro**: simple; familiar; low operational overhead; easy to debug.
**Con**: clients get raw upstream shapes; versioning concerns; no single contract.

## Decision

Proposal C, implemented as a custom Go service rather than Kong (Kong's licensing complexity not worth it for our size). First version handles: JWT auth validation, rate limiting per API key, request routing with path rewriting, basic observability (request logs, Prometheus metrics endpoint).

GraphQL revisited in 6 months after the routing layer is stable.

Reference: https://microservices.io/patterns/apigateway.html
