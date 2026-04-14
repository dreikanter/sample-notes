# Go concurrency patterns — working notes

Working through the event pipeline architecture (see [[20231212_1337]]) required actually understanding goroutines and channels rather than just using them. These are the patterns that clarified things.

## Goroutines and channels — the basics

```go
ch := make(chan int, 10) // buffered channel, capacity 10

go func() {
    for i := 0; i < 10; i++ {
        ch <- i
    }
    close(ch)
}()

for v := range ch {
    fmt.Println(v)
}
```

Range over a channel reads until the channel is closed. Always close channels from the sender side, never the receiver.

## Select for multiple channels

```go
select {
case msg := <-msgChan:
    process(msg)
case <-ctx.Done():
    return ctx.Err()
case <-ticker.C:
    flush()
}
```

`select` blocks until one case is ready, then executes it. If multiple cases are ready simultaneously, one is chosen at random. The `ctx.Done()` pattern is how you implement cancellation cleanly.

## Worker pool

```go
func workerPool(jobs <-chan Job, results chan<- Result, n int) {
    var wg sync.WaitGroup
    for i := 0; i < n; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            for j := range jobs {
                results <- process(j)
            }
        }()
    }
    wg.Wait()
    close(results)
}
```

The WaitGroup ensures all workers finish before results is closed. A common bug: closing results before all workers are done.

Reference: [The Go Memory Model](https://go.dev/ref/mem) and [Go by Example](https://gobyexample.com/goroutines) for practical patterns.
