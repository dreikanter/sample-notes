# Go concurrency basics

## Goroutines

```go
go func() {
    fmt.Println("running concurrently")
}()

// With a named function
go processItem(item)
```

Goroutines are cheap (a few KB stack, grows as needed). You can run thousands.

## Channels

```go
// Unbuffered: send blocks until receive
ch := make(chan int)

// Buffered: send blocks only when full
ch := make(chan int, 10)

// Send and receive
ch <- 42
val := <-ch

// Range over channel (until closed)
for v := range ch {
    fmt.Println(v)
}

// Close
close(ch)  // receivers see zero value after close, not panic
```

## Select

```go
select {
case msg := <-ch1:
    fmt.Println("from ch1:", msg)
case msg := <-ch2:
    fmt.Println("from ch2:", msg)
case <-time.After(time.Second):
    fmt.Println("timeout")
default:
    fmt.Println("no message ready")
}
```

## sync.WaitGroup

```go
var wg sync.WaitGroup
for _, item := range items {
    wg.Add(1)
    go func(i Item) {
        defer wg.Done()
        process(i)
    }(item)
}
wg.Wait()
```

## sync.Mutex

```go
var mu sync.Mutex
var counter int

mu.Lock()
counter++
mu.Unlock()

// Or use defer
func increment() {
    mu.Lock()
    defer mu.Unlock()
    counter++
}
```

## sync.Once

```go
var once sync.Once
var instance *DB

func GetDB() *DB {
    once.Do(func() {
        instance = initDB()
    })
    return instance
}
```

## Common patterns

**Fan-out:** One goroutine sends work to many workers via channel.
**Fan-in:** Many goroutines send results to one aggregating goroutine.
**Pipeline:** Chain of goroutines where output of one is input to next.

The Go blog on concurrency patterns: https://go.dev/blog/pipelines
