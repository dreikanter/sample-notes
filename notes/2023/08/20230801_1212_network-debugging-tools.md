---
title: Network debugging tools — reference
slug: network-debugging-tools
tags: [networking, sysadmin, reference]
description: A reference for the network debugging tools I actually use.
---

# Network debugging tools — reference

**`curl` — HTTP debugging**

```bash
# Verbose output — show headers
curl -v https://api.example.com/endpoint

# Include response headers only
curl -I https://example.com

# POST with JSON body
curl -X POST https://api.example.com/data \
  -H "Content-Type: application/json" \
  -d '{"key":"value"}'

# Follow redirects, show timing
curl -L -w "@curl-format.txt" https://example.com
```

Timing format file for `curl -w`:

```
time_namelookup:  %{time_namelookup}\n
time_connect:     %{time_connect}\n
time_appconnect:  %{time_appconnect}\n
time_pretransfer: %{time_pretransfer}\n
time_starttransfer: %{time_starttransfer}\n
time_total:       %{time_total}\n
```

**`dig` — DNS lookups**

```bash
dig example.com A          # A record
dig example.com MX         # Mail records
dig @8.8.8.8 example.com  # Use specific resolver
dig +short example.com     # Just the answer
```

**`ss` — socket statistics (replaces `netstat`)**

```bash
ss -tlnp       # Listening TCP ports with process names
ss -s          # Summary statistics
ss -tnp state established  # All established connections
```

**`tcpdump` — packet capture**

```bash
tcpdump -i eth0 port 443          # HTTPS traffic
tcpdump -i any host 10.0.0.1     # Traffic to/from IP
tcpdump -w capture.pcap           # Write to file for Wireshark
```

**`mtr` — continuous traceroute**

Better than traceroute for identifying packet loss at specific hops.

```bash
mtr --report --report-cycles 100 example.com
```

[The Linux Command Line networking chapter](https://linuxcommand.org/tlcl.php)
