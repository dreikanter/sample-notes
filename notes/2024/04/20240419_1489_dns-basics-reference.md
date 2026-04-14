# DNS basics reference

Always have to look these up. Keeping a consolidated reference.

## Record types

```
A       — hostname → IPv4 address
AAAA    — hostname → IPv6 address
CNAME   — alias → canonical hostname (cannot coexist with other records at apex)
MX      — mail exchange server, with priority
NS      — nameservers for a zone
TXT     — arbitrary text (used for SPF, DKIM, domain verification)
SOA     — Start of Authority, zone metadata
PTR     — reverse DNS, IP → hostname
SRV     — service location (host, port, priority, weight)
CAA     — Certificate Authority Authorization
```

## Querying

```bash
# Basic lookup
dig example.com
dig example.com A
dig example.com MX

# Specific nameserver
dig @8.8.8.8 example.com

# Reverse lookup
dig -x 1.2.3.4

# Short output
dig +short example.com

# Trace the full resolution path
dig +trace example.com

# Check nameservers
dig NS example.com

# TTL info
dig +ttl example.com
```

## TTL considerations

TTL (Time To Live) in seconds: how long recursive resolvers cache the answer. During a migration:
1. Lower TTL to 300 (5 min) 24-48 hours before the change
2. Make the DNS change
3. Wait TTL seconds before decommissioning old servers
4. Raise TTL back to 3600+ after migration confirmed

## Common troubleshooting

**Record not propagating:** Check TTL on old record; wait for expiry. Confirm change at authoritative nameserver with `@auth-ns`.

**MX issue:** Check that MX record points to a hostname (A record), not an IP directly.

**CNAME loop:** CNAME points to another CNAME which points back — resolvers detect this but it's a misconfiguration.

**SPF for email:** TXT record like `v=spf1 include:_spf.google.com ~all`

DNS propagation check tool: https://dnschecker.org/
