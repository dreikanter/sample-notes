---
title: Network fundamentals I keep revisiting
slug: network-fundamentals
tags: [networking, reference, backend, http]
---

# Network fundamentals I keep revisiting

Things that come up in debugging and monitoring contexts and that I have to re-derive every time. Reference: [Beej's Guide to Network Programming](https://beej.us/guide/bgnet/).

## The TCP handshake

Three-way handshake establishes a connection:
1. **SYN**: client sends a packet with SYN flag set, random sequence number
2. **SYN-ACK**: server acknowledges with SYN+ACK, its own sequence number
3. **ACK**: client acknowledges; connection established

This is why connection establishment adds latency. TLS adds another 1-2 round trips on top. HTTP/2 and HTTP/3 optimise this.

## TCP vs UDP

**TCP**: reliable, ordered delivery. Retransmits lost packets. Used for: HTTP, database connections, SSH, email.

**UDP**: fire and forget. No delivery guarantee, no ordering. Used for: DNS (primarily), video streaming, online gaming, VoIP — cases where low latency matters more than reliable delivery.

## DNS resolution

```
Browser cache → OS cache → resolving resolver (ISP or 1.1.1.1)
  → root nameserver → TLD nameserver (.com, .org)
  → authoritative nameserver for the domain
```

A full resolution can take 8+ DNS queries. TTLs determine how long results are cached. Low TTLs (300s) allow faster DNS changes but increase resolver load.

## Common port numbers to remember

| Port | Protocol |
|------|----------|
| 22 | SSH |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 443 | HTTPS |
| 5432 | PostgreSQL |
| 6379 | Redis |
| 27017 | MongoDB |

## CIDR notation

`192.168.1.0/24` means: the first 24 bits are the network address, the last 8 bits are host addresses. This gives 256 addresses (254 usable). `/16` gives 65,536 addresses; `/32` is a single host.

## What happens when you type a URL

1. DNS resolution → IP address
2. TCP connection to port 443
3. TLS handshake
4. HTTP request sent
5. Server processes, sends HTTP response
6. Browser parses HTML, requests sub-resources (CSS, JS, images)
7. Sub-resources trigger more TCP connections (or reuse via HTTP/2 multiplexing)
