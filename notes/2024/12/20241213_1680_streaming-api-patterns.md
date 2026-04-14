---
title: Streaming API patterns — SSE and WebSockets
slug: streaming-api-patterns
tags: [api, streaming, websockets, sse, backend]
---

# Streaming API patterns — SSE and WebSockets

Notes on when to use Server-Sent Events (SSE) versus WebSockets. See [MDN on SSE](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events) and [MDN on WebSockets](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket).

## Server-Sent Events (SSE)

One-way: server to client only. Uses standard HTTP. Text-based protocol with automatic reconnection built in.

**When to use:**
- Real-time dashboard updates
- Activity feeds and notifications
- Streaming LLM responses
- Log tailing
- Progress updates for long operations

**Server (Node.js example):**
```javascript
app.get('/events', (req, res) => {
  res.writeHead(200, {
    'Content-Type': 'text/event-stream',
    'Cache-Control': 'no-cache',
    'Connection': 'keep-alive',
  });

  const send = (data) => res.write(`data: ${JSON.stringify(data)}\n\n`);
  const interval = setInterval(() => send({ time: Date.now() }), 1000);
  req.on('close', () => clearInterval(interval));
});
```

**Client:**
```javascript
const es = new EventSource('/events');
es.onmessage = (event) => console.log(JSON.parse(event.data));
```

## WebSockets

Bidirectional, full-duplex. Persistent TCP connection. More overhead to set up and maintain.

**When to use:**
- Chat applications
- Collaborative editing
- Real-time gaming
- Any application where the client needs to send frequent messages to the server

## Comparison

| Feature | SSE | WebSocket |
|---------|-----|-----------|
| Direction | Server → client | Bidirectional |
| Protocol | HTTP | WS (upgrade from HTTP) |
| Auto-reconnect | Yes (built-in) | No (implement yourself) |
| Proxy/firewall support | Better | Varies |
| Binary support | No | Yes |

**Default recommendation**: if you only need server-to-client updates, SSE is simpler, works through standard HTTP proxies, and handles reconnection automatically. Reach for WebSockets only when you genuinely need bidirectional communication.
