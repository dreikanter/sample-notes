# Go concurrency patterns

Working notes on Go concurrency. Channels and goroutines are the main tools; these are the patterns I use.

## Worker pool

```go
func workerPool(jobs <-chan Job, results chan<- Result, workerCount int) {
    var wg sync.WaitGroup
    for i := 0; i < workerCount; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            for job := range jobs {
                results <- process(job)
            }
        }()
    }
    wg.Wait()
    close(results)
}
```

Send to `jobs`, range receives until channel closed. Workers exit naturally. `WaitGroup` ensures all workers finish before closing results.

## Pipeline

```go
func generate(nums ...int) <-chan int {
    out := make(chan int)
    go func() {
        for _, n := range nums {
            out <- n
        }
        close(out)
    }()
    return out
}

func square(in <-chan int) <-chan int {
    out := make(chan int)
    go func() {
        for n := range in {
            out <- n * n
        }
        close(out)
    }()
    return out
}
```

Each stage returns a channel. Compose: `square(generate(1,2,3,4))`.

## Fan-out, fan-in

Distribute work across multiple goroutines, merge results into one channel. The `merge` function uses a WaitGroup and a goroutine per input channel.

## Context for cancellation

```go
ctx, cancel := context.WithTimeout(context.Background(), 5*time.Second)
defer cancel()

select {
case result := <-doWork(ctx):
    // use result
case <-ctx.Done():
    // timed out or cancelled
}
```

Pass `ctx` through the call tree; check `ctx.Done()` at blocking points.

Reference: https://go.dev/blog/pipelines
Go concurrency guide: https://go.dev/doc/effective_go#concurrency
