---
title: Go channels — patterns and gotchas
slug: go-channels-patterns
tags: [go, golang, concurrency, programming]
description: Notes on Go channel usage patterns accumulated while starting the notes CLI project.
---

# Go channels — patterns and gotchas

Working notes from building the notes CLI prototype (see [[20251115_1997]]). The concurrency model keeps surprising me.

## Basic patterns

```go
// Unbuffered: sender blocks until receiver is ready
ch := make(chan int)

// Buffered: sender blocks only when buffer is full
ch := make(chan int, 10)

// Send and receive
ch <- value    // send
value := <-ch  // receive (blocks until value available)
```

## Done channel pattern (cancellation)

```go
func worker(done <-chan struct{}) {
    for {
        select {
        case <-done:
            return
        default:
            // do work
        }
    }
}

done := make(chan struct{})
go worker(done)
close(done)  // signals all workers listening on done
```

Closing a channel broadcasts to all receivers—key for fan-out cancellation.

## Fan-out, fan-in

```go
// Fan-out: distribute work across workers
func fanOut(in <-chan Work, workers int) []<-chan Result {
    outputs := make([]<-chan Result, workers)
    for i := 0; i < workers; i++ {
        outputs[i] = process(in)
    }
    return outputs
}

// Fan-in: merge multiple channels into one
func merge(cs ...<-chan Result) <-chan Result {
    out := make(chan Result)
    var wg sync.WaitGroup
    for _, c := range cs {
        wg.Add(1)
        go func(ch <-chan Result) {
            defer wg.Done()
            for v := range ch {
                out <- v
            }
        }(c)
    }
    go func() { wg.Wait(); close(out) }()
    return out
}
```

## Common gotchas

- Sending to a closed channel panics
- Receiving from a nil channel blocks forever
- Goroutine leaks: always ensure goroutines can exit; use context or done channels

Reference: [Go concurrency patterns talk by Rob Pike](https://talks.golang.org/2012/concurrency.slide)
