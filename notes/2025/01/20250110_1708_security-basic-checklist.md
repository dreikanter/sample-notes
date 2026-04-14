# Web application security — basic checklist

A baseline security review checklist for web applications. Not comprehensive — the [OWASP Top 10](https://owasp.org/www-project-top-ten/) is the definitive reference.

## Authentication

- [ ] Passwords hashed with bcrypt, Argon2, or scrypt (never MD5, SHA-1, or SHA-256 alone)
- [ ] Rate limiting on login attempts
- [ ] Account lockout or CAPTCHA after repeated failures
- [ ] Secure password reset flow (token-based, time-limited, single-use)
- [ ] HTTPS enforced everywhere — no HTTP fallback
- [ ] Session tokens are random, sufficient length (≥128 bits), invalidated on logout

## Authorization

- [ ] Every endpoint checks that the authenticated user is allowed to access the resource
- [ ] Server-side enforcement only — never trust client-supplied roles or permissions
- [ ] Principle of least privilege in service accounts and API keys

## Input validation and injection

- [ ] Parameterised queries everywhere — no string concatenation for SQL
- [ ] HTML output escaped to prevent XSS
- [ ] File uploads validated (type, size, not executed by the server)
- [ ] User-supplied redirects validated against an allowlist

## HTTP headers

```
Content-Security-Policy: default-src 'self'
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: camera=(), microphone=()
```

Use [securityheaders.com](https://securityheaders.com) to check your headers.

## Secrets management

- [ ] No secrets in source code, git history, or environment variable dumps
- [ ] Secrets in a vault or environment-specific config (not in `.env` files committed to git)
- [ ] API keys rotated on suspected compromise

## Dependencies

- [ ] Automated dependency scanning (Dependabot, Snyk)
- [ ] Regular updates, especially for security patches

## Monitoring

- [ ] Logging sufficient to detect and investigate incidents
- [ ] Alerting on suspicious patterns (unusual login rates, error spikes)
