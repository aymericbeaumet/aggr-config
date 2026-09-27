---
title: Go Concurrency Distilled
link: https://antonz.org/go-concurrency-distilled/
source: hnrss-org-frontpage
published: 2026-09-26T14:34:49Z
updated: 2026-09-26T14:34:49Z
first_seen: 2026-09-27T10:43:41.928516245Z
authors:
- chmaynard
summary: 'Article URL: https://antonz.org/go-concurrency-distilled/ Comments URL: https://news.ycombinator.com/item?id=49856988 Points: 224 # Comments: 85'
content: extracted
html: 2026-09-26-go-concurrency-distilled.html
preview:
  file: 2026-09-26-go-concurrency-distilled.preview-f349c028b434.webp
  width: 256
  height: 128
  color: '#0f0f0f'
images:
- source: https://antonz.org/go-concurrency-distilled/cover.png
  original:
    file: 2026-09-26-go-concurrency-distilled.image-11c2cf7b0d82.png
    width: 1280
    height: 640
  variants:
  - file: 2026-09-26-go-concurrency-distilled.image-fa1a52d5a07c.webp
    width: 320
    height: 160
  - file: 2026-09-26-go-concurrency-distilled.image-ba63fb4e1d0d.webp
    width: 640
    height: 320
  - file: 2026-09-26-go-concurrency-distilled.image-78c5a150d9a4.webp
    width: 960
    height: 480
  - file: 2026-09-26-go-concurrency-distilled.image-cabd25cbeef8.webp
    width: 1280
    height: 640
  color: '#000000'
---

This mini-book provides a brief overview of many concurrency topics in Go. Each topic comes with interactive examples — feel free to experiment with them by changing the code and clicking *Run*. There's also a [PDF version](https://github.com/nalgeon/go-conc-distilled) with static examples.

> This is a quick refresher on Go concurrency, not a beginner's guide. If you want to learn concurrency from the ground up with practical exercises, check out my other book — [Gist of Go: Concurrency](https://antonz.org/go-concurrency).

The book is AI-free.

[Goroutines](https://antonz.org/go-concurrency-distilled/#goroutines) • [Channels](https://antonz.org/go-concurrency-distilled/#channels) • [Select](https://antonz.org/go-concurrency-distilled/#select) • [Pipelines](https://antonz.org/go-concurrency-distilled/#pipelines) • [Time](https://antonz.org/go-concurrency-distilled/#time) • [Context](https://antonz.org/go-concurrency-distilled/#context) • [Wait groups](https://antonz.org/go-concurrency-distilled/#wait-groups) • [Data races](https://antonz.org/go-concurrency-distilled/#data-races) • [Race conditions](https://antonz.org/go-concurrency-distilled/#race-conditions) • [Mutexes](https://antonz.org/go-concurrency-distilled/#mutexes) • [Semaphores](https://antonz.org/go-concurrency-distilled/#semaphores) • [Signaling](https://antonz.org/go-concurrency-distilled/#signaling) • [Run once](https://antonz.org/go-concurrency-distilled/#run-once) • [Object pool](https://antonz.org/go-concurrency-distilled/#object-pool) • [Atomics](https://antonz.org/go-concurrency-distilled/#atomics) • [Testing](https://antonz.org/go-concurrency-distilled/#testing) • [Scheduling](https://antonz.org/go-concurrency-distilled/#scheduling) • [Diagnostics](https://antonz.org/go-concurrency-distilled/#diagnostics) • [Final thoughts](https://antonz.org/go-concurrency-distilled/#final-thoughts)

## Goroutines

The foundation of concurrency in Go is *goroutines* – functions started with the `go` keyword:

```go
func main() {
    var wg sync.WaitGroup
    wg.Add(2)
    go func() {
        defer wg.Done()
        fmt.Println("worker 1")
    }()
    go func() {
        defer wg.Done()
        fmt.Println("worker 2")
    }()
    wg.Wait()
}
```

```text
worker 2
worker 1
```

The Go runtime juggles these goroutines and distributes them among operating system threads running on CPU cores. Compared to OS threads, goroutines are lightweight, so you can create hundreds or thousands of them.

Goroutines are completely independent. The main function is also a goroutine, but it starts implicitly when the program starts. When `main` ends, other goroutines also shut down.

We use a *wait group* (`sync.WaitGroup`) to wait for goroutines to finish in the example above. A wait group has a counter inside. Calling `Add(n)` increments it by `n`, while `Done()` decrements it by one. `Wait()` blocks the calling goroutine (in this case, main) until the counter reaches zero. This way, main waits for both workers to finish before it exits.

`WaitGroup.Go` automatically increments the wait group counter, runs a function in a goroutine, and decrements the counter when it's done:

```go
func main() {
    var wg sync.WaitGroup
    wg.Go(func() {
        fmt.Println("worker 1")
    })
    wg.Go(func() {
        fmt.Println("worker 2")
    })
    wg.Wait()
}
```

```text
worker 2
worker 1
```

## Channels

Goroutines can pass values to each other through *channels*. A channel is like a window where one goroutine can throw something and another can catch it:

```go
func main() {
    messages := make(chan string)

    go func() { messages <- "ping" }()

    msg := <-messages
    fmt.Println(msg)
}
```

```text
ping
```

Sending a value through a channel is a synchronous operation. When the sending goroutine writes a value to the channel (`ch <- val`), it blocks and waits for someone to receive that value (`<-ch`). Only then does it continue.

### Output channel

Returning an output channel from a function and filling it within an internal goroutine is a common pattern in Go. This allows the caller to receive values through the channel while the owning function retains control of it:

```go
func generate(start, stop int) chan int {
    out := make(chan int)
    go func() {
        for i := start; i < stop; i++ {
            out <- i
        }
    }()
    return out
}
```

### Closing a channel

To signal readers that all data has been sent, the writer goroutine *closes* the channel with `close()`:

```go
func generate(start, stop int) chan int {
    out := make(chan int)
    go func() {
        defer close(out)
        for i := start; i < stop; i++ {
            out <- i
        }
    }()
    return out
}
```

The reader checks the channel's status with a second value ("comma OK") when reading:

```go
func main() {
    in := generate(5, 10)
    for {
        num, ok := <-in
        if !ok {
            break
        }
        fmt.Print(num, " ")
    }
}
```

```text
5 6 7 8 9
```

While the channel is open, the reader receives the next value and a `true` status. If the channel is closed, the reader gets a zero value and a `false` status.

A channel can only be closed once. Closing it again or writing to a closed channel causes a panic.

The only reason to close a channel is to signal to its readers that all data has been sent. If this isn't important to the readers, then you don't need to close it. When a channel is no longer used, Go's garbage collector will free its resources, whether it's closed or not.

### Channel iteration

`range` automatically reads the next value from the channel and checks if it's closed. If the channel is closed, it exits the loop:

```go
func main() {
    nums := generate(5, 10)
    for n := range nums {
        fmt.Print(n, " ")
    }
}
```

```text
5 6 7 8 9
```

Range over a channel returns a single value, not a pair, unlike range over a slice.

### Directional channels

You can protect yourself from accidental write/close errors by setting the channel direction. Channels can be:

- `chan` (bidirectional): for reading and writing (default);
- `chan<-` (send-only): for writing only;
- `<-chan` (receive-only): for reading only.

You can't read from a send-only channel or write to a receive-only channel (nor can you close it).

Channels are usually initialized for both reading and writing, and specified as directional in function parameters. Go automatically converts a regular channel to a directional one:

```go
stream := make(chan int)

go func(in chan<- int) {
    in <- 42
}(stream)

func(out <-chan int) {
    fmt.Println(<-out)
}(stream)
```

```text
42
```

### Buffered channels

*Buffered* channels work like a FIFO queue with a fixed-size buffer for storing values.

As long as the buffer has free space, writing to the channel doesn't block the goroutine. Similarly, as long as the buffer contains values, reading from the channel doesn't block the goroutine:

```go
stream := make(chan int, 3)
stream <- 11
stream <- 12
stream <- 13

fmt.Println(<-stream)
fmt.Println(<-stream)
```

```text
11
13
```

By default, if you don't specify a buffer size, a channel is *unbuffered* (buffer size equals zero).

Buffered channels work with the built-in `len()` and `cap()` functions:

```go
stream := make(chan int, 3)
stream <- 11
fmt.Println(cap(stream), len(stream))
```

```text
3 1
```

Reading from a closed buffered channel returns values from the buffer and a `true` status. Once all values are taken, it returns a zero value and a `false` status, like a regular channel:

```go
stream := make(chan int, 1)
stream <- 11
close(stream)

val, ok := <-stream
fmt.Println(val, ok)
// 11 true

val, ok = <-stream
fmt.Println(val, ok)
// 0 false
```

```text
11 true
0 false
```

### nil channel

Like any type in Go, channels have a zero value, which is `nil`.

Writing to or reading from a nil channel blocks the goroutine indefinitely:

```go
var stream chan int

go func() {
    // blocks forever
    stream <- 1
}()

// blocks forever
<-stream
```

Closing a nil channel causes a panic:

```go
var stream chan int
close(stream)
// panic: close of nil channel
```

## Select

The *select* statement is somewhat like `switch`, but specifically designed for channels. Here's what it does:

- Checks which cases are not blocked.
- If multiple cases are ready, randomly selects one to execute.
- If all cases are blocked and there is a default case, executes it.
- If all cases are blocked and there is no default case, waits until one is ready.

Select is used to manage data flow in pipelines:

```go
// merge sends values from in1 and in2 to the output channel.
func merge(in1, in2 <-chan int) <-chan int {
    out := make(chan int)
    go func() {
        defer close(out)
        for in1 != nil || in2 != nil {
            select {
            case val1, ok := <-in1:
                if ok { out <- val1 } else { in1 = nil }
            case val2, ok := <-in2:
                if ok { out <- val2 } else { in2 = nil }
            }
        }
    }()
    return out
}

// Suppose we send 10..12 to in1, 20..22 to in2,
// and call merge(in1, in2)
```

```text
10 11 20 12 21 22
```

To cancel goroutines:

```go
// process modifies values from in and send them to out
// until in is exhausted or cancel is closed.
func process(cancel chan struct{}, in <-chan int) <-chan int {
    out := make(chan int)
    go func() {
        for val := range in {
            select {
            case out <- val*10:
            case <-cancel:
                fmt.Println("canceled")
                return
            }
        }
    }()
    return out
}

// Suppose we send values 11 and 12 to in
// and then call close(cancel)
```

```text
110
120
canceled
```

For non-blocking operations:

```go
// multiplier returns a function that multiplies
// the input by 10 and sends it to the channel
// or returns an error if the channel is busy.
func multiplier(ch chan<- int) func(n int) error {
    return func(n int) error {
        select {
        case ch <- n*10:
            return nil
        default:
            return errors.New("busy")
        }
    }
}

func main() {
    nums := make(chan int, 1)
    multiply := multiplier(nums)

    err := multiply(11)
    fmt.Println(<-nums, err)
    // 110 <nil>

    err = multiply(12)
    fmt.Println(<-nums, err)
    // 120 <nil>

    err = multiply(13)
    err = multiply(14)
    fmt.Println(err)
    // busy
}
```

```text
110 <nil>
120 <nil>
busy
```

And for much more.

## Pipelines

A *pipeline* is a sequence of operations where each step takes input data, processes it in a specific way, and outputs it. The input and output of each operation is a channel.

A typical pipeline looks like this:

- *Reader*: Reads input data from a file, database, or network.
- *N processors*: Transform, filter, aggregate, or enrich data using external sources.
- *Writer*: Writes the processed data to a file, database, or network.

```go
func read[T any]() <-chan T {
    out := make(chan T)
    go func() {
        defer close(out)
        for {
            // read data from somewere
            data := // ...
            out <- data
        }
    }()
    return out
}

func process[T any](in <-chan T) <-chan T {
    out := make(chan T)
    go func() {
        defer close(out)
        for inData := range in {
            // process the data
            outData = // ...
            out <- outData
        }
    }()
    return out
}

func write[T any](in <-chan T) <-chan struct{} {
    done := make(chan struct{})
    go func() {
        defer close(done)
        for data := range in {
            // write the data
        }
    }()
    return done
}
```

### Output channel

A goroutine can signal other goroutines that it has finished its work using an *output channel*:

```go
func generate(start, stop int) <-chan int {
    out := make(chan int)
    go func() {
        defer close(out)
        for i := start; i < stop; i++ {
            out <- i
        }
    }()
    return out
}

func main() {
    nums := generate(5, 10)
    for n := range nums {
        fmt.Print(n, " ")
    }
}
```

```text
5 6 7 8 9
```

### Done channel

If a goroutine doesn't need to return results, it can signal completion using a *done channel*:

```go
func work() <-chan struct{} {
    done := make(chan struct{})
    go func() {
        defer close(done)
        fmt.Println("work done")
    }()
    return done
}

func main() {
    done := work()
    <-done
}
```

```text
work done
```

### Cancel channel

To terminate a goroutine early, a calling goroutine can use a *cancel channel*:

```go
func generate(cancel chan struct{}, n int) <-chan int {
    out := make(chan int)
    go func() {
        defer close(out)
        for i := 1; i <= n; i++ {
            select {
            case out <- i:
            case <-cancel:
                return
            }
        }
    }()
    return out
}

func main() {
    cancel := make(chan struct{})
    defer close(cancel)

    nums := generate(cancel, 10)
    fmt.Println(<-nums)
    fmt.Println(<-nums)
    fmt.Println(<-nums)
}
```

```text
1
2
3
```

### Error handling

There are three approaches to error handling in concurrent pipelines.

➊ Return on the first error:

```go
// calculate produces answers for the given numbers.
func process(in <-chan int) (<-chan int, <-chan error) {
	out := make(chan Answer)
	errc := make(chan error, 1)
	go func() {
		defer close(out)
		for n := range in {
			ans, err := fetchAnswer(n)
			if err != nil {
				errc <- err  // return with error
				return
			}
			out <- ans
		}
		errc <- nil          // return with nil
	}()
	return out, errc
}
```

➋ Use a result type:

```go
// Result contains an answer or an error.
type Result struct {
	answer int
	err    error
}

// calculate produces answers for the given numbers.
func calculate(in <-chan int) <-chan Result {
	out := make(chan Result)
	go func() {
		defer close(out)
		for n := range in {
			ans, err := fetchAnswer(n)
			out <- Result{ans, err}  // return answer + error
		}
	}()
	return out
}
```

➌ Collect errors separately:

```go
// calculate produces answers for the given numbers.
func calculate(in <-chan int, errc chan<- error) <-chan int {
	out := make(chan Answer)
	go func() {
		defer close(out)
		for n := range in {
			ans, err := fetchAnswer(n)
			if err == nil {
				out <- ans   // send answer
			} else {
				errc <- err  // or error
			}
		}
	}()
	return out
}
```

## Time

Besides handling date and time, the `time` package offers tools for managing time-sensitive operations in concurrent programs.

### After

`time.After()` returns a channel that is initially empty, but receives a value after the timeout period. It's useful for timing out operations:

```go
// withTimeout executes a function with a given timeout.
func withTimeout(timeout time.Duration, fn func()) error {
    done := make(chan struct{})
    go func() {
        defer close(done)
        fn()
    }()

    // blocks until fn completes or the timer expires,
    // whichever happens first
    select {
    case <-done:
        return nil
    case <-time.After(timeout):
        return errors.New("timeout")
    }
}
```

`withTimeout()` waits for `fn()` to complete, but thanks to `time.After()`, it won't wait longer than the `timeout` duration:

```go
func main() {
    var err error

    // completes in time
    err = withTimeout(
        50*time.Millisecond,
        func() { fmt.Println("work done") },
    )
    fmt.Println("err =", err)

    // gets canceled on timeout
    err = withTimeout(
        50*time.Millisecond,
        func() {
            time.Sleep(100 * time.Millisecond)
            fmt.Println("work done")
        },
    )
    fmt.Println("err =", err)
}
```

```text
work done
err = <nil>
err = timeout
```

### Timer

A *timer* (`time.Timer`) is a structure with a `C` channel to which it sends the current time when it triggers (expires). Timers are useful for planning future executions:

```go
done := make(chan struct{})

timer := time.NewTimer(50 * time.Millisecond)
go func() {
    eventTime := <-timer.C  // blocks for 50ms
    fmt.Println("work done at", eventTime)
    close(done)
}()

<-done
```

```text
work done at 2009-11-10 23:00:00.05
```

`Stop()` stops the timer and returns `true` if it hasn't expired yet, and `false` otherwise:

```go
// timer expires after 50ms
timer := time.NewTimer(50 * time.Millisecond)
go func() {
    eventTime := <-timer.C
    fmt.Println("work done at", eventTime)
}()

// after 10ms, the timer hasn't expired yet
time.Sleep(10 * time.Millisecond)

if timer.Stop() {
    fmt.Println("execution canceled")
} else {
    fmt.Println("too late to cancel")
}
```

```text
execution canceled
```

It's often more convenient to use the `time.AfterFunc()` wrapper function. It waits for duration `d` and then executes function `f`:

```go
done := make(chan struct{})
work := func() {
    fmt.Println("work done")
    close(done)
}

// executes work after 50ms
time.AfterFunc(50*time.Millisecond, work)
<-done
```

```text
work done
```

`time.AfterFunc()` returns a timer that you can cancel before execution starts:

```go
// executes the function after 50ms
timer := time.AfterFunc(50*time.Millisecond, func() {})

// after 10ms, the timer hasn't expired yet
time.Sleep(10 * time.Millisecond)

if timer.Stop() {
    fmt.Println("execution canceled")
}
```

```text
execution canceled
```

If a timer is used in a loop, it's better to create a single timer and *reset* it instead of creating a new instance on each iteration:

```go
// consumer reads tokens from the input channel and alerts
// if a value does not appear in a channel after an hour.
func consumer(in <-chan token) {
    const timeout = time.Hour
    timer := time.NewTimer(timeout)
    for {
        timer.Reset(timeout)
        select {
        case <-in:
            // do stuff
        case <-timer.C:
            // log warning
        }
    }
}

// Suppose we send 10,000 values to the in channel
// and measure memory usage.
```

```text
Memory used: 4 KB, # allocations: 6
```

### Ticker

A *ticker* is like a timer, but it keeps firing until you stop it. Tickers are useful for executing periodic tasks:

```go
// fires every 50ms
ticker := time.NewTicker(50 * time.Millisecond)
defer ticker.Stop()

go func() {
    for {
        // waits for ticker to fire on each iteration
        at := <-ticker.C
        fmt.Println("work done at", at)
    }
}()

// enough time for the ticker to fire 3 times
time.Sleep(160*time.Millisecond)
ticker.Stop()
```

```text
work done at 2009-11-10 23:00:00.05
work done at 2009-11-10 23:00:00.10
work done at 2009-11-10 23:00:00.15
```

`NewTicker(d)` creates a ticker that sends the current time to the channel `C` at interval `d`. You must stop the ticker eventually with `Stop()` to free up resources.

If the channel reader can't keep up with the ticker, the ticker will skip ticks.

## Context

The main purpose of *context* is to cancel operations, either manually or by timeout/deadline.

The function accepts a context and uses its `Done()` channel to listen for cancellation:

```go
// work performs a task for 50 ms unless canceled.
// Returns an error when canceled.
func work(ctx context.Context) error {
    done := make(chan struct{})

    go func() {
        time.Sleep(50 * time.Millisecond)
        fmt.Println("work done")
        close(done)
    }()

    select {
    case <-done:
        return nil
    case <-ctx.Done():
        return ctx.Err()
    }
}
```

Cancel manually (`context.Canceled` error):

```go
func main() {
    // empty context
    ctx := context.Background()
    // manual canellation context
    ctx, cancel := context.WithCancel(ctx)
    defer cancel()

    done := make(chan struct{})
    go func() {
        // takes 50 ms unless canceled
        err := work(ctx)
        fmt.Println("err =", err)
        close(done)
    }()

    // cancels after 10 ms
    time.Sleep(10 * time.Millisecond)
    cancel()
    <-done
}
```

```text
err = context canceled
```

Cancel by timeout (`context.DeadlineExceeded` error):

```go
func main() {
    ctx := context.Background()
    // cancels after 10 ms
    ctx, cancel := context.WithTimeout(ctx, 10*time.Millisecond)
    defer cancel()

    done := make(chan struct{})
    go func() {
        // takes 50 ms unless canceled
        err := work(ctx)
        fmt.Println("err =", err)
        close(done)
    }()

    <-done
}
```

```text
err = context deadline exceeded
```

Cancel by deadline (`context.DeadlineExceeded` error):

```go
func main() {
    ctx := context.Background()
    // cancels at now + 10 ms
    deadline := time.Now().Add(10 * time.Millisecond)
    ctx, cancel := context.WithDeadline(ctx, deadline)
    defer cancel()

    done := make(chan struct{})
    go func() {
        // takes 50 ms unless canceled
        err := work(ctx)
        fmt.Println("err =", err)
        close(done)
    }()

    <-done
}
```

```text
err = context deadline exceeded
```

Context is layered. A context object is immutable. To add new properties to a context, a new (child) context is created based on the old (parent) context. The shorter timeout between the parent and child contexts always wins. The child context can only shorten the parent's timeout, not extend it:

```go
func main() {
    // parent context with a 100 ms timeout
    const dur100ms = 100 * time.Millisecond
    parentCtx, cancel := context.WithTimeout(context.Background(), dur100ms)
    defer cancel()

    // child context with a 10 ms timeout
    const dur10ms = 10 * time.Millisecond
    childCtx, cancel := context.WithTimeout(parentCtx, dur10ms)
    defer cancel()

    // now the work gets canceled
    err := work(childCtx)
    fmt.Println("err =", err)
}
```

```text
err = context deadline exceeded
```

Multiple cancels are safe. You can call `cancel()` on the context as many times as you want. The first cancel will work, and the rest will be ignored.

You can specify a custom cancellation cause using `context.WithCancelCause()`, `context.WithTimeoutCause()` and `context.WithDeadlineCause()`. This cause is accessible through `context.Cause()`:

```go
ctx, cancel := context.WithCancelCause(context.Background())
cancel(errors.New("the night is dark"))
fmt.Println(context.Cause(ctx))
```

```text
the night is dark
```

You can register a function to execute when the context is canceled with `context.AfterFunc()`:

```go
ctx, cancel := context.WithCancel(context.Background())
cleanup := func() { fmt.Println("cleanup") }
context.AfterFunc(ctx, cleanup)
cancel()
time.Sleep(10 * time.Millisecond)
```

```text
cleanup
```

Context can pass additional information about a call using `context.WithValue()`, which creates a context with a value for a specific key. But it's generally better to avoid passing values in context. It's better to use explicit parameters or custom structs instead.

## Wait groups

The `sync.WaitGroup` type lets you wait for one or more goroutines to finish:

```go
const n = 10
var wg sync.WaitGroup
wg.Add(n)
for range n {
    go func() {
        defer wg.Done()
        fmt.Print(".")
    }()
}
wg.Wait()
```

```text
..........
```

A `WaitGroup` doesn't know anything about the goroutines it manages. It works with an internal counter. Calling `wg.Add(1)` increments the counter by one, while `wg.Done()` decrements it. `wg.Wait()` blocks the calling goroutine until the counter reaches zero.

The `Go` method combines `Add`, starting a goroutine, and `Done`:

```go
var wg sync.WaitGroup
for range 10 {
    wg.Go(func() {
        fmt.Print(".")
    })
}
wg.Wait()
```

```text
..........
```

All methods are safe to use from multiple goroutines.

Normally, all `Add` calls happen before `Wait`. But technically, there's nothing stopping you from doing some of the `Add` calls before `Wait` and some after (from another goroutine).

You can call `Wait` from multiple goroutines. They will all block until the group's counter reaches zero.

## Data races

A data race happens when multiple goroutines access shared data, and at least one of them modifies it. We need to protect the data from this kind of concurrent access.

A data race doesn't always cause a runtime panic. That's why Go provides a special tool called the race detector. You can turn it on with the `race` flag, which works with the `test`, `run`, `build`, and `install` commands.

```go
var total int

// There's a data race on total.
var wg sync.WaitGroup
wg.Go(func() { total++ })
wg.Go(func() { total++ })
wg.Wait()

fmt.Println("total:", total)
```

```text
total: 2
```

```text
go run -race main.go
```

```text
==================
WARNING: DATA RACE
...
2
Found 1 data race(s)
```

Channels are safe for concurrent reading and writing, and they don't cause data races.

Ways to prevent data races:

- Avoid concurrent data modification (typically by using channels).
- Synchronize access with mutexes.
- Use only atomic operations.

## Race conditions

A race condition happens when an unpredictable order of operations from multiple goroutines leads to an incorrect system state:

```go
// There's a race condition when working with balance.
withdraw := func(amount int) {
    if getBalance() < amount {
        return
    }
    time.Sleep(time.Millisecond)
    setBalance(getBalance() - amount)
}

setBalance(50)

var wg sync.WaitGroup
wg.Go(func() { withdraw(40) })
wg.Go(func() { withdraw(40) })
wg.Wait()

fmt.Println("balance:", getBalance())
```

```text
balance: -30
```

If individual operations are concurrent-safe, Go's race detector won't find any issues. Because of this, it doesn't catch race conditions:

```text
go run -race main.go
```

```text
balance: -30
```

You can't fully eliminate uncertainty in a concurrent environment. Events will happen in an unpredictable order — that's just how concurrency works. However, you can prevent a race condition — often by protecting a composite operation with a mutex:

```go
var mu sync.Mutex
withdraw := func(amount int) {
    mu.Lock()
    defer mu.Unlock()

    if getBalance() < amount {
        return
    }
    time.Sleep(time.Millisecond)
    setBalance(getBalance() - amount)
}

setBalance(50)

var wg sync.WaitGroup
wg.Go(func() { withdraw(40) })
wg.Go(func() { withdraw(40) })
wg.Wait()

fmt.Println("balance:", getBalance())
```

```text
balance: 10
```

### Compare-and-set

Sometimes you can prevent a race condition without using mutexes by applying an atomic compare-and-set operation or one of its flavors:

```go
// CompareAndSet changes the value to new if the current value equals old.
// Returns true if the value was changed.
CompareAndSet(old, new any) bool

// CompareAndSwap changes the value to new if the current value equals old.
// Returns the old value.
CompareAndSwap(old, new any) any

// CompareAndDelete deletes the value if the current value equals old.
// Returns true if the value was deleted.
CompareAndDelete(old any) bool

// etc
```

The idea is always the same:

- Check if the assumed (old) state matches reality.
- If it does, change the state to new.
- If not, do nothing.

## Mutexes

The `sync.Mutex` type protects shared data and parts of your code from being accessed concurrently:

```go
var total int
var mu sync.Mutex

var wg sync.WaitGroup
for range 100 {
    wg.Go(func() {
        mu.Lock()
        time.Sleep(time.Millisecond)
        total++
        mu.Unlock()
    })
}
wg.Wait()
```

```text
total: 100
```

The mutex guarantees that only one goroutine can run the code between `Lock()` and `Unlock()` at a time.

A mutex is used in these situations:

- When multiple goroutines are modifying the same data.
- When one goroutine is modifying the data and others are reading it.

If all goroutines are only reading the data, you don't need a mutex.

### TryLock

The `TryLock` method tries to lock the mutex, just like a regular `Lock`. But if it can't, it returns `false` right away instead of blocking the goroutine:

```go
var total int
var mu sync.Mutex

var wg sync.WaitGroup
for range 100 {
    wg.Go(func() {
        if !mu.TryLock() {
            return
        }
        defer mu.Unlock()
        time.Sleep(time.Millisecond)
        total++
    })
}
wg.Wait()
```

```text
total: 1
```

### RWMutex

The `sync.RWMutex` type distinguishes between readers and writers. It provides two sets of methods:

- `Lock` / `Unlock` lock and unlock the mutex for both reading and writing.
- `RLock` / `RUnlock` lock and unlock the mutex for reading only.

```go
var total int
var mu sync.RWMutex
var wg sync.WaitGroup

// 10 writers.
for range 10 {
    wg.Go(func() {
        mu.Lock()
        defer mu.Unlock()
        time.Sleep(time.Millisecond)
        total++
    })
}

// 10 readers.
for range 10 {
    wg.Go(func() {
        // Try switching from RLock/RUnlock to Lock/Unlock
        //and see how it affects the elapsed time.
        mu.RLock()
        defer mu.RUnlock()
        time.Sleep(time.Millisecond)
        _ = total
    })
}

wg.Wait()
```

```text
elapsed: 10ms
```

Here's how it works:

- If a goroutine locks the mutex with `Lock()`, other goroutines will be blocked if they try to use `Lock()` or `RLock()`.
- If a goroutine locks the mutex with `RLock()`, other goroutines can also lock it with `RLock()` without being blocked.
- If at least one goroutine has locked the mutex with `RLock()`, other goroutines will be blocked if they try to use `Lock()`.

This creates a "single writer, multiple readers" setup.

### Locker

Both `sync.Mutex` and `sync.RWMutex` implement the same `sync.Locker` interface:

```go
type Locker interface {
    Lock()
    Unlock()
}
```

By using `Locker` instead of a specific mutex type, you can build components that don't depend on a specific lock implementation. This lets the client decide which lock to use.

### Channel as mutex

You can use a channel instead of a mutex to protect shared data:

```go
var total int
lock := make(chan struct{}, 1)

var wg sync.WaitGroup
wg.Go(func() {
    lock <- struct{}{}
    defer func() { <-lock }()
    total++
})
wg.Go(func() {
    lock <- struct{}{}
    defer func() { <-lock }()
    total++
})
wg.Wait()
```

```text
total: 2
```

## Semaphores

A semaphore is like a container with N available slots and two operations: *acquire* to take a slot and *release* to free a slot. Here are the semaphore rules:

- Calling acquire takes a free slot.
- If there are no free slots, acquire blocks the goroutine that called it.
- Calling release frees up a previously taken slot.
- If there are any goroutines blocked on acquire when release is called, one of them will immediately take the freed slot and unblock.

You can implement a simple semaphore with a buffered channel, where N is the channel's size. To acquire the semaphore, send a value into the channel. To release it, take a value from the channel:

```go
// Try changing nConc and see how the elapsed time changes.
const nConc = 4
const nCalls = 100
sema := make(chan struct{}, nConc)

var wg sync.WaitGroup
for range nCalls {
    sema <- struct{}{} // acquire
    wg.Go(func() {
        defer func() { <-sema }() // release
        time.Sleep(time.Millisecond) // do some work
    })
}
wg.Wait()
```

```text
elapsed: 25ms
```

For more complex situations, use the `golang.org/x/sync/semaphore` package.

### Rendezvous

A rendezvous lets two goroutines wait for each other:

- There are two goroutines — G1 and G2 — and each one can signal that it's ready.
- If G1 signals but G2 hasn't yet, G1 blocks and waits.
- If G2 signals but G1 hasn't yet, G2 blocks and waits.
- When both have signaled, they both unblock and continue running.

You can implement a simple rendezvous with a wait group:

```go
var rend sync.WaitGroup
rend.Add(2)

var wg sync.WaitGroup
wg.Go(func() {
    fmt.Println("before rendezvous")
    rend.Done()
    rend.Wait()
    fmt.Println("after rendezvous")
})
wg.Go(func() {
    fmt.Println("before rendezvous")
    rend.Done()
    rend.Wait()
    fmt.Println("after rendezvous")
})
wg.Wait()
```

```text
before rendezvous
before rendezvous
after rendezvous
after rendezvous
```

### Barrier

A barrier is a general case of a rendezvous. It lets N goroutines wait for each other:

- The barrier has a counter (starting at 0) and a threshold N.
- Each goroutine that reaches the barrier increases the counter by 1.
- The barrier blocks any goroutine that reaches it.
- Once the counter reaches N, the barrier unblocks all waiting goroutines.

You can implement a simple barrier with a wait group:

```go
const n = 4
var bar sync.WaitGroup
bar.Add(n)

var wg sync.WaitGroup
for range n {
    wg.Go(func() {
        fmt.Println("before the barrier")
        bar.Done()
        bar.Wait()
        fmt.Println("after the barrier")
    })
}
wg.Wait()
```

```text
before the barrier
before the barrier
before the barrier
before the barrier
after the barrier
after the barrier
after the barrier
after the barrier
```

## Signaling

The `sync.Cond` (conditional variable) type lets one goroutine signal to another that it's ready, and lets the other goroutine wait for that signal.

A `Cond` includes a mutex and has two methods — `Wait` and `Signal`.

- `Wait` unlocks the mutex and suspends the goroutine until it receives a signal.
- `Signal` wakes the goroutine that is waiting on `Wait`.
- When `Wait` wakes up, it locks the mutex again.

```go
cond := sync.NewCond(&sync.Mutex{})
done := false

var wg sync.WaitGroup
wg.Go(func() {
    cond.L.Lock()
    fmt.Println("G1 is ready to signal")
    done = true
    cond.Signal()
    cond.L.Unlock()
})
wg.Go(func() {
    cond.L.Lock()
    for !done {
        cond.Wait()
    }
    fmt.Println("G2 received the signal")
    cond.L.Unlock()
})
wg.Wait()
```

```text
G1 is ready to signal
G2 received the signal
```

If there are multiple waiting goroutines when `Signal` is called, only one of them will be resumed. If there are no waiting goroutines, `Signal` does nothing.

You can also use the `Broadcast` method. While `Signal` wakes up only one goroutine waiting on `Cond.Wait`, the `Broadcast` method wakes up all such goroutines.

You can signal with a channel:

```go
signal := make(chan struct{}, 1)

go func() {
    // do something
    signal <- struct{}{}
}()

go func() {
    <-signal
    // do something
}()
```

And broadcast too:

```go
broadcast := make(chan struct{})

go func() {
    // do something
    close(broadcast)
}()

go func() {
    <-broadcast
    // do something
}()

go func() {
    <-broadcast
    // do something
}()
```

Broadcasting with a condition variable is limited: it only sends a signal, not the actual data, and it only works once. With channels, you can build a publish/subscribe system that doesn't have these limitations:

```go
type Publisher struct {
    sbox []chan int // subscription channels
    mu   sync.Mutex // protects the state
}

func (p *Publisher) Subscribe() <-chan int {
    p.mu.Lock()
    defer p.mu.Unlock()
    sub := make(chan int, 1)
    p.sbox = append(p.sbox, sub)
    return sub
}

func (p *Publisher) Broadcast(v int) {
    p.mu.Lock()
    defer p.mu.Unlock()
    for _, sub := range p.sbox {
        select {
        case sub <- v:
        default:
        }
    }
}
```

## Run once

The `sync.Once` type makes sure that the given function runs only once. If multiple goroutines call `Once.Do` at the same time, only one will run the function, while the others will wait until it returns:

```go
total := 0
initState := func() {
    total += 1
}

var once sync.Once

var wg sync.WaitGroup
wg.Go(func() {
    once.Do(initState)
    // do something
})
wg.Go(func() {
    once.Do(initState)
    // do something
})
wg.Wait()
```

```text
total: 1
```

`Once` is perfect for one-time initialization or cleanup in a concurrent environment.

Besides the `Once` type, the `sync` package also includes three convenience once-functions:

```go
// Calls f only once.
func (o *Once) Do(f func())

// Returns a function that calls f only once.
func OnceFunc(f func()) func()

// Returns a function that calls f only once
// and returns the value from that first call.
func OnceValue[T any](f func() T) func() T

// Returns a function that calls f only once
// and returns the pair of values from that first call.
func OnceValues[T1, T2 any](f func() (T1, T2)) func() (T1, T2)
```

## Object pool

The `sync.Pool` type helps reuse memory instead of allocating it every time, which reduces the load on the garbage collector:

```go
pool := sync.Pool{
    New: func() any {
        buf := make([]byte, 1024)
        return &buf
    },
}

// Only allocates 4*1024 B, despite 4000 loop iterations.
var wg sync.WaitGroup
for range 4 {
    wg.Go(func() {
        for range 1000 {
            buf := pool.Get().(*[]byte)
            sink = buf
            pool.Put(buf)
        }
    })
}
wg.Wait()
```

```text
Memory allocated: 4 KB
```

`Get` takes an item from the pool. If there are no available items, it creates a new one using `New` (which we have to define ourselves, since the pool doesn't know anything about the items it creates). `Put` returns an item back to the pool.

Things to keep in mind:

- `New` should return a pointer, not a value, to reduce memory copying and avoid extra allocations.
- The pool has no size limit. If you start 1000 more goroutines that all call `Get` at the same time, 1000 more buffers will be allocated.
- After an item is returned to the pool with `Put`, you shouldn't use it anymore (since another goroutine might already have taken and started using it).

## Atomics

An operation without synchronization can only be truly atomic if it translates to a single processor instruction. Such operations don't need locks and won't cause issues when called concurrently (even the write operations).

There are only a few atomics, and they're all found in the `sync/atomic` package:

```text
Int32     Bool
Int64     Value
Uint32    Pointer
Uint64
```

Each atomic type provides the following methods:

- `Load` reads the value of a variable.
- `Store` sets a new value.
- `Swap` sets a new value (like `Store`) and returns the old one.
- `CompareAndSwap` sets a new value only if the current value is still what you expect it to be.

```go
var n atomic.Int32
n.Store(10)
swapped := n.CompareAndSwap(10, 42)
fmt.Println("CompareAndSwap 10 -> 42:", swapped)
fmt.Println("n =", n.Load())
```

```text
CompareAndSwap 10 -> 42: true
n = 42
```

Numeric types also provide an `Add` method that increments the value by the specified amount.

All methods are either translated into a single CPU instruction or are otherwise guaranteed to be atomic, so they are safe to use from multiple goroutines.

The composition of atomics is always non-atomic:

```go
var delta atomic.Int32
var counter atomic.Int32

func increment() {
    // Not atomic; causes a race condition.
    delta.Add(1)
    sleep(10)
    counter.Add(delta.Load())
}

// After 100 concurrent increments,
// the final value is NOT guaranteed.
```

```text
counter = 9386
```

A bulletproof way to make a composite operation atomic and prevent race conditions is to use a mutex:

```go
var delta int32
var counter int32
var mu sync.Mutex

func increment() {
    // Atomic; doesn't cause a race condition.
    mu.Lock()
    delta += 1
    sleep(10)
    counter += delta
    mu.Unlock()
}

// After 100 concurrent increments, the final value is guaranteed:
// counter = 1+2+...+100 = 5050
```

```text
counter = 5050
```

Sometimes you can use an atomic type instead of a mutex to exit early:

```go
type Gate struct {
    closed atomic.Bool
}

func (g *Gate) Close() {
    if !g.closed.CompareAndSwap(false, true) {
        return // ignore repeated calls
    }
    // The gate is closed.
    // We can free resources now.
}
```

## Testing

If your concurrent program uses channels or custom types with synchronization methods like `Wait`, you can use those in your tests. This way, your tests won't be much more complicated than if the code were synchronous:

```go
// Calc calculates something asynchronously.
func Calc() <-chan int {
    out := make(chan int, 1)
    go func() {
        out <- 42
    }()
    return out
}
```

```go
func Test(t *testing.T) {
    // Wait for the Calc goroutine to finish.
    got := <-Calc()
    if got != 42 {
        t.Errorf("got: %v; want: 42", got)
    }
}
```

```text
PASS
```

If there aren't any suitable synchronization "handles" in the code you're testing, you can use the `synctest` package. It exports two functions:

```go
func Test(t *testing.T, f func(*testing.T))
func Wait()
```

`synctest.Test` runs an isolated bubble. The bubble uses a fake clock, and you can manually control goroutine synchronization with `synctest.Wait`.

`synctest.Wait` blocks until all goroutines in the bubble — except the one that called `Wait` — have either finished or are durably blocked. This lets you wait for a specific goroutine to finish or get blocked, so you can check the program's state:

```go
// NewProc starts the calculation.
func NewProc() *Proc {
    p := &Proc{done: make(chan struct{})}
    go func() {
        p.res = 42
        <-p.done // (X)
        p.res = 0
    }()
    return p
}
```

```go
func Test(t *testing.T) {
    synctest.Test(t, func(t *testing.T) {
        p := NewProc()
        defer p.Stop()

        // Wait for the goroutine to block at point X.
        synctest.Wait()
        if got := p.Res(); got != 42 {
            t.Fatalf("got %v, want 42", got)
        }
    })
}
```

```text
PASS
```

The fake clock in `synctest.Test` move forward only if: ➊ all goroutines in the bubble are durably blocked; ➋ there's a future moment when at least one goroutine will unblock; and ➌ `synctest.Wait` isn't running. Thanks to this, time-dependent tests run instantly:

```go
// Calc processes a value from the input channel.
// Times out if no input is received after 3 seconds.
func Calc(in chan int) (int, error) {
    select {
    case v := <-in:
        return v * 2, nil
    case <-time.After(3 * time.Second):
        return 0, ErrTimeout
    }
}
```

```go
func Test(t *testing.T) {
    synctest.Test(t, func(t *testing.T) {
        ch := make(chan int)
        got, err := Calc(ch) // runs instantly

        if err != ErrTimeout {
            t.Errorf("got: %v; want: %v", err, ErrTimeout)
        }
        if got != 0 {
            t.Errorf("got: %v; want: 0", got)
        }
    })
}
```

```text
PASS
```

The following operations durably block a goroutine:

- A blocking send or receive on a channel created within the bubble.
- A blocking select statement where every case is a channel created within the bubble.
- Calling `Cond.Wait`.
- Calling `WaitGroup.Wait` if all `WaitGroup.Add` calls were made inside the bubble.
- Calling `time.Sleep`.

Blocking on mutexes, I/O, or system calls is not considered durable, and the `synctest` bubble can't handle them.

## Scheduling

At the hardware level, CPU cores are responsible for running parallel tasks.

At the operating system level, a thread is the basic unit of execution. There are usually many more threads than CPU cores, so the operating system's scheduler decides which threads to run and which ones to pause.

At the Go runtime level, a goroutine is the basic unit of execution. The runtime scheduler runs a fixed number of OS threads, often one per CPU core. There can be many more goroutines than threads, so the scheduler decides which goroutines to run on the available threads and which ones to pause. The scheduler keeps switching between goroutines to make sure each one gets a turn to run on a thread, instead of waiting in line forever.

```text
  CPU                  OS                   Go runtime
┌──────────┐  run on ┌──────────┐  run on ┌────────────┐
│ Cores    │ <────── │ Threads  │ <────── │ Goroutines │
└──────────┘         └──────────┘         └────────────┘
```

This is how Go handles concurrency.

**Goroutine scheduler**

The goroutine scheduler's job is to run M goroutines on N operating system threads, where M can be much larger than N. Here's a very simplified version of it's algorithm:

- If there's a free thread, assign it a goroutine from the queue.
- If a running goroutine gets blocked (for example, while reading from a channel), put it back in the queue and assign a different goroutine to the thread.
- If a running goroutine gets stuck in a syscall, start a new thread to run other goroutines until the blocked goroutine finishes the syscall.
- Check the running goroutines every 10 ms. Preempt long-running goroutines and return them to the queue to prevent starvation.

```text
┌─────┐┌─────┐┌─────┐┌─────┐
│ G17 ││ G18 ││ G19 ││ G20 │                        queue
└─────┘└─────┘└─────┘└─────┘

┌─────┐      ┌─────┐      ┌─────┐      ┌─────┐
│ G15 │      │ G16 │      │ G13 │      │ G14 │      running
└─────┘      └─────┘      └─────┘      └─────┘
  │            │            │            │
┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐
│ Thread E │ │ Thread F │ │ Thread C │ │ Thread D │
└──────────┘ └──────────┘ └──────────┘ └──────────┘

┌─────┐      ┌─────┐
│ G11 │      │ G12 │                                syscalls
└─────┘      └─────┘
  │            │
┌──────────┐ ┌──────────┐
│ Thread A │ │ Thread B │
└──────────┘ └──────────┘
```

The number of threads running Go code is controlled by the `GOMAXPROCS` environment variable or the `runtime.GOMAXPROCS` function.

A goroutine is a structure that starts out using about 2 KB of memory, mostly for its stack. The stack can grow if needed. Since goroutines are so lightweight, you can run tens of thousands or even hundreds of thousands of them on a small machine.

## Diagnostics

To troubleshoot concurrent programs in production, we use metrics, profiling, and tracing.

**Metrics** show how the Go runtime is performing, like how much heap memory it uses or how long garbage collection pauses take. Each metric has a unique name and a value, which can be a number or a histogram.

You can use the `runtime/metrics` package to get a complete list of metrics or check the values of specific ones:

```go
samples := []metrics.Sample{
    {Name: "/sched/gomaxprocs:threads"},
    {Name: "/sched/goroutines:goroutines"},
}
metrics.Read(samples)

for _, s := range samples {
    fmt.Printf("%s: %v\n", s.Name, s.Value.Uint64())
}
```

```text
/sched/gomaxprocs:threads: 8
/sched/goroutines:goroutines: 1
```

In practice, people rarely do this manually. Instead, all metrics are automatically exported using Prometheus or OpenTelemetry libraries.

**Profiling** helps you understand exactly what the program is doing, what resources it uses, and where in the code this happens. Go uses a sampling profiler that's suitable for production.

The most commonly used profiles are CPU, which shows how much processor time each function uses, and heap, which shows how much heap memory each function uses. Goroutine, block, and mutex profiles help identify problems related to concurrency.

The easiest way to add a profiler to your app is by using the `net/http/pprof` package. To collect a profile with the given name, call the `/debug/pprof/{name}` endpoint. To view the collected profile, use the `go tool pprof` utility:

```text
go tool pprof -proto \
  "http://localhost:6060/debug/pprof/profile?seconds=N" > cpu.pprof
go tool pprof -http=localhost:8080 cpu.pprof
```

You can also profile manually:

```go
// CPU profile.
file, _ := os.Create("cpu.prof")
defer file.Close()
pprof.StartCPUProfile(file)
defer pprof.StopCPUProfile()
// ...
```

```go
// Any other profile.
file, _ := os.Create(name + ".prof")
defer file.Close()
pprof.Lookup(name).WriteTo(file, 0)
```

**Tracing** records certain types of events while the program is running, mainly those related to concurrency and memory. When the profiling server from the `net/http/pprof` package is running, call the `/debug/pprof/trace` endpoint to collect a trace. To view the results, use the `go tool trace` utility.

You can also collect a trace manually:

```go
file, _ := os.Create("trace.out")
defer file.Close()
trace.Start(file)
defer trace.Stop()
// ...
```

You can set up automatic tracing with a sliding window that's limited by size or duration. This is called "flight recording". It lets you always keep a recent trace available in case something goes wrong:

```go
cfg := trace.FlightRecorderConfig{
    MinAge:   5 * time.Second,
    MaxBytes: 3 << 20, // 3MB
}
rec := trace.NewFlightRecorder(cfg)
rec.Start()
defer rec.Stop()
```

## Final thoughts

We've covered a number of Go tools for writing concurrent programs:

- Goroutines for running concurrent tasks.
- Channels and select as flexible communication tools.
- Timers and tickers for working with time.
- Context for canceling operations.
- Wait groups for synchronizing goroutines.
- Mutexes to prevent race conditions.
- Condition variables for signaling events.
- Once for safe one-time initialization.
- Pools to reduce garbage collector load.
- Atomic operations.

If you like the book, please recommend it to your friends or colleagues. If you're interested, check out my other [books](https://antonz.org/#books) and [projects](https://antonz.org/tags/projects/).

I'm glad you finished the book. Thank you, and I'll see you next time!

[★ Subscribe](https://antonz.org/subscribe/) to keep up with new posts.
