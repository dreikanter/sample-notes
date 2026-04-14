---
title: Understanding TCP flow control
slug: understanding-tcp-flow-control
tags: [networking, tcp, reference]
description: Notes on TCP's sliding window and flow control mechanics
public: true
---

# Understanding TCP flow control

TCP flow control prevents a fast sender from overwhelming a slow receiver. The receiver advertises a receive window (rwnd) in each ACK — this is the number of bytes it can currently buffer. The sender must keep unacknowledged data below rwnd at all times.

## Sliding window mechanics

The sender maintains three pointers into the byte stream:

1. Last byte acknowledged (everything before this is done)
2. Last byte sent (between 1 and 3 is in-flight)
3. Last byte the sender is allowed to send (1 + rwnd)

As ACKs arrive, pointer 1 advances and the window slides right. If rwnd drops to zero, the sender stops and starts probing with 1-byte segments every few seconds to detect when the receiver recovers.

## Congestion control is separate

Flow control (rwnd) handles receiver capacity. Congestion control (cwnd) handles network capacity. The sender's effective window is `min(rwnd, cwnd)`. They operate in parallel and use different signals — rwnd comes from the receiver's ACKs; cwnd is maintained locally and shaped by packet loss or ECN marks.

## Nagle's algorithm

Small writes get buffered until the in-flight data is acknowledged. This reduces packet count but adds latency. Disabled with `TCP_NODELAY`, which is common for interactive applications (SSH, game servers, financial feeds).

For deeper reading on the full TCP state machine: https://datatracker.ietf.org/doc/html/rfc9293

Practical experiments with `ss -ti` on Linux will show rwnd, cwnd, and retransmit counts live on any open socket.
