---
title: Meeting notes — auth spec security review
slug: meeting-notes-security-review
tags: [work, security, meetings]
---

# Meeting notes — auth spec security review

**Date:** 2025-03-08
**Attendees:** Me, Selin, plus external security consultant (Dariusz)
**Purpose:** Review auth spec from [[20250211_1734]] before implementation

## Issues flagged

### High priority

1. **Token storage guidance missing:** The spec says "store tokens securely" without specifying. Dariusz wants explicit guidance: HttpOnly cookies for browser clients, secure storage APIs for mobile, never localStorage. Added to spec.

2. **No PKCE requirement for public clients:** If any of our clients are mobile or SPA-based, they need PKCE (Proof Key for Code Exchange) to prevent authorization code interception. We haven't decided yet whether we're building a public client flow, but if we do, PKCE is mandatory.

### Medium priority

3. **Refresh token rotation:** Our current spec says single-use rotation, which is correct. Dariusz confirmed and added: include a grace period of ~5 seconds for network retries. Refresh tokens occasionally get consumed by a request that never delivers the response; the grace period prevents forced logouts.

4. **Rate limiting on the token endpoint:** Not in the current spec. Need to add — the token endpoint is a brute-force target. Limit to 10 requests per minute per IP, with exponential backoff on repeated failures.

### Low priority

5. **Audit logging:** Should log token issuance, refresh, and revocation events. Not blocking for v2 beta but should be in v2.1.

## Decision

All high and medium items addressed before implementation begins. Aiming to restart implementation next Monday.

OWASP OAuth cheat sheet: [https://cheatsheetseries.owasp.org/cheatsheets/OAuth_Cheat_Sheet.html](https://cheatsheetseries.owasp.org/cheatsheets/OAuth_Cheat_Sheet.html)
