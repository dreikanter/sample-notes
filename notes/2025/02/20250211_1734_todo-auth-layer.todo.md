# Auth layer spec — todo

Work items for the auth layer spec referenced in [[20250204_1723]]. Needs to be done before end of February to keep API v2 on track.

## Must complete this week

- [ ] Define token format: JWT vs opaque, decide and document rationale
- [ ] Specify refresh token rotation policy (decide: single-use or sliding window)
- [ ] Document scope model — what scopes exist, how they map to endpoints
- [ ] Write up error response format for auth failures (standardize across v2)

## Next week

- [ ] Internal review with Marcus on the scope model
- [ ] Security review checklist — go through OWASP OAuth 2.0 guidance
- [ ] Draft migration guide for v1 API key holders

## Questions to resolve

- Are we supporting service-to-service tokens (machine credentials) in v1 of the auth system, or deferring?
- What's the token TTL? Current assumption is 1 hour access, 30 days refresh, but Priya wanted to discuss.
- How do we handle token revocation? Blocklist approach needs a fast lookup store.

## References

- [https://datatracker.ietf.org/doc/html/rfc6749](https://datatracker.ietf.org/doc/html/rfc6749) — OAuth 2.0 RFC
- [https://owasp.org/www-project-api-security/](https://owasp.org/www-project-api-security/) — OWASP API security
- [https://jwt.io/introduction](https://jwt.io/introduction) — JWT intro

## Done

- [x] Confirm v2 auth timeline with PM (done 2025-02-04)
- [x] Locate previous auth design docs from v0 era
