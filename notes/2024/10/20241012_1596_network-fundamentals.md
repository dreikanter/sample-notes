# Network fundamentals I keep forgetting

Not a tutorial. The specific concepts I have to look up more than I should.

## TCP vs UDP

**TCP:** Connection-oriented, ordered, reliable delivery, flow control, congestion control. The overhead is the guarantee.

**UDP:** Connectionless, no ordering guarantee, no delivery guarantee, lower latency. Use when losing a packet is acceptable (live video, DNS queries, gaming).

Why DNS uses UDP: you're sending a tiny request and expecting a small response. The TCP handshake overhead would slow the whole internet down noticeably. If the response exceeds 512 bytes, DNS falls back to TCP.

## The three-way handshake

SYN → SYN-ACK → ACK

Before a TCP connection exists, both sides exchange these. The server listens, client sends SYN, server sends SYN-ACK (I got your request, here's my sequence number), client sends ACK. Now data flows.

Connection teardown: four packets (FIN → ACK → FIN → ACK) because each side closes independently.

## CIDR notation

`192.168.1.0/24` means: the first 24 bits are the network, the last 8 are host addresses. A /24 gives you 256 addresses (254 usable — first and last reserved).

Common ones:
- /32: single host
- /24: 256 addresses (class C equivalent)
- /16: 65,536 addresses
- /8: 16.7 million addresses

## What a subnet mask is

`/24` in subnet mask notation is `255.255.255.0`. The ones mark network bits, the zeros mark host bits. Binary: `11111111.11111111.11111111.00000000`.

[Julia Evans' networking zine](https://jvns.ca/networking/) is the best approachable resource on this.
