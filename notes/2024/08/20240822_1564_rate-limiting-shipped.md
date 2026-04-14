# Rate limiting shipped

The feature went out in production deploy at 2pm today after three weeks of implementation. Zero incidents so far (monitoring for 24 hours before declaring it stable).

The numbers from the load test:
- p50 latency added: 1.8ms
- p99 latency added: 7.2ms
- Redis call success rate: 99.97%

The latency overhead is within the originally-stated target of <10ms at p99. The 0.03% Redis failure rate is higher than I'd like — current behavior is fail open (requests go through), which was the deliberate choice for now. Will investigate the failures — likely network hiccups rather than correctness issues.

The Lua script atomicity worked correctly under load. Had a brief moment of doubt during testing when I saw some inconsistent counts, traced it to test harness concurrency rather than the implementation.

One thing I'd do differently: I wrote the metrics instrumentation last, as an afterthought. It should have been written first. Observability is a feature, not a finishing touch.

What's next: the bulk export feature begins sprint 16. That one is mine from design through delivery.

Also: writing a postmortem-style retrospective on the rate-limiting project. Even when things go well, the process retro is worth doing. Template from [Google's SRE book](https://sre.google/sre-book/postmortem-culture/).
