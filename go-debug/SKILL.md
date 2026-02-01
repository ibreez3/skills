---
name: go-debug
description: Go bug analysis, diagnosis, and fixing. Expert guidance for identifying, locating, and resolving Go bugs including panics, race conditions, memory leaks, performance issues, and common anti-patterns. Use this skill when debugging Go code.
license: MIT
---

This skill provides systematic approach to analyzing, locating, and fixing bugs in Go codebases. It covers common bug patterns, debugging techniques, tools, and best practices for troubleshooting.

The user invokes this skill when:
- Investigating a bug or unexpected behavior
- Analyzing panic or crash reports
- Debugging race conditions or deadlocks
- Investigating memory leaks
- Diagnosing performance issues
- Fixing error handling problems

## Bug Analysis Framework

### Phase 1: Bug Classification

First, identify the bug type:

| Bug Category | Symptoms | Common Causes |
|--------------|----------|---------------|
| **Panic/Crash** | Stack trace, abrupt exit | Nil pointer, slice bounds, type assertion |
| **Race Condition** | Non-deterministic behavior, data corruption | Unsynchronized shared access |
| **Deadlock** | Program hangs, goroutines blocked | Channel misuse, mutex ordering |
| **Memory Leak** | Memory grows over time | Goroutine leaks, unclosed resources |
| **Logic Error** | Wrong output, incorrect behavior | Algorithm bugs, edge cases |
| **Resource Leak** | File descriptors, connections | Missing close/defer |
| **Performance** | Slow execution, high CPU | Inefficient algorithms, unnecessary allocations |
| **Concurrency** | Liveness issues, starvation | Improper goroutine management |

### Phase 2: Information Gathering

Collect diagnostic information:

```bash
# Enable verbose logging
go run -tags debug main.go

# Check Go version
go version

# View build info
go version -m ./binary

# Race detector
go run -race main.go
go test -race ./...

# CPU profiling
go test -cpuprofile=cpu.prof
go tool pprof cpu.prof

# Memory profiling
go test -memprofile=mem.prof
go tool pprof mem.prof

# Goroutine profiling
curl http://localhost:6060/debug/pprof/goroutine
go tool pprof -http=:8080 http://localhost:6060/debug/pprof/goroutine
```

### Phase 3: Root Cause Analysis

Apply appropriate debugging strategy based on bug type.

## Common Bug Patterns & Solutions

### 1. Nil Pointer Dereference

**Symptoms:**
```
panic: runtime error: invalid memory address or nil pointer dereference
```

**Detection:**
```bash
# Stack trace shows the exact line
panic: runtime error: invalid memory address or nil pointer dereference
[signal SIGSEGV: segmentation violation code=0x1 addr=0x8 pc=0x...]

goroutine 1 [running]:
main.(*User).GetName(...)
        /path/to/file.go:42
main.main()
        /path/to/main.go:10
```

**Common Causes:**
- Uninitialized pointers
- Missing error checks
- Incorrect interface type assertions
- Uncast nil values

**Solutions:**
```go
// DON'T: Assume non-nil
func process(user *User) {
    fmt.Println(user.Name)  // Panics if user is nil
}

// DO: Check for nil
func process(user *User) {
    if user == nil {
        return nil, fmt.Errorf("user is nil")
    }
    fmt.Println(user.Name)
}

// DON'T: Ignore errors
user, _ := getUser(id)
user.GetName()  // Panics if user is nil

// DO: Always check errors
user, err := getUser(id)
if err != nil {
    return err
}
user.GetName()

// DON'T: Unsafe type assertion
var i interface{} = "hello"
n := i.(int)  // Panics!

// DO: Use comma-ok pattern
n, ok := i.(int)
if !ok {
    return fmt.Errorf("not an int")
}
```

**Prevention:**
- Use `nil` analysis in IDE
- Enable `staticcheck` linter
- Always check error returns
- Use panic recovery in top-level handlers

### 2. Slice/Index Out of Range

**Symptoms:**
```
panic: runtime error: index out of range [N] with length M
```

**Detection:**
```bash
# Stack trace shows problematic index
panic: runtime error: index out of range [5] with length 3
```

**Common Causes:**
- Off-by-one errors
- Empty slices
- Concurrent modification
- Incorrect loop bounds

**Solutions:**
```go
// DON'T: Assume slice has elements
first := items[0]  // Panics if empty

// DO: Check length
if len(items) == 0 {
    return nil
}
first := items[0]

// DON'T: Hardcode bounds
for i := 0; i <= len(items); i++ {
    fmt.Println(items[i])  // Panics at i == len
}

// DO: Use range
for i, item := range items {
    fmt.Println(i, item)
}

// DON'T: Unsafe slicing
sub := items[5:10]  // Panics if len < 10

// DO: Validate bounds
end := 10
if end > len(items) {
    end = len(items)
}
sub := items[5:end]
```

### 3. Race Conditions

**Symptoms:**
- Non-deterministic test failures
- Intermittent crashes
- Data corruption
- Valgrind/drd warnings

**Detection:**
```bash
# Run with race detector
go run -race main.go
go test -race ./...

# Output shows race location:
==================
WARNING: DATA RACE
Write at 0x... by goroutine 7:
  main.updateCounter()
      /path/file.go:15 +0x...

Previous write at 0x... by goroutine 6:
  main.updateCounter()
      /path/file.go:15 +0x...
==================
```

**Common Patterns:**

```go
// BUG: Unsynchronized access
var counter int

func increment() {
    counter++  // DATA RACE!
}

// FIX: Use atomic or mutex
var counter int64

func increment() {
    atomic.AddInt64(&counter, 1)
}

// OR
var (
    counter int
    mu      sync.Mutex
)

func increment() {
    mu.Lock()
    defer mu.Unlock()
    counter++
}

// BUG: Race on slice append
var items []string

func addItem(item string) {
    items = append(items, item)  // DATA RACE!
}

// FIX: Protect with mutex
var (
    items []string
    mu    sync.Mutex
)

func addItem(item string) {
    mu.Lock()
    defer mu.Unlock()
    items = append(items, item)
}

// BUG: Check-then-act race
if counter == 0 {  // RACE!
    counter = 1
}

// FIX: Atomic compare-and-swap
for {
    old := atomic.LoadInt64(&counter)
    if old != 0 {
        break
    }
    if atomic.CompareAndSwapInt64(&counter, old, 1) {
        break
    }
}

// OR use mutex
mu.Lock()
defer mu.Unlock()
if counter == 0 {
    counter = 1
}
```

**Channel Race Patterns:**

```go
// BUG: Close and send race
go func() {
    ch <- value
}()
close(ch)  // Potential panic: send on closed channel

// FIX: Wait for send completion
go func() {
    ch <- value
    close(ch)
}()

// OR use wait group
var wg sync.WaitGroup
wg.Add(1)
go func() {
    defer wg.Done()
    ch <- value
}()
wg.Wait()
close(ch)
```

### 4. Deadlocks

**Symptoms:**
- Program hangs indefinitely
- Goroutine dump shows all goroutines blocked

**Detection:**
```bash
# Get goroutine dump
curl http://localhost:6060/debug/pprof/goroutine?debug=2

# Look for:
# - All goroutines in chan send/recv
# - Mutex lock cycles
# - No goroutines runnable
```

**Common Patterns:**

```go
// BUG: Self-deadlock
mu.Lock()
mu.Lock()  // Deadlock!

// FIX: Use RWMutex or restructure
var rw sync.RWMutex
rw.Lock()
rw.Lock()  // Still deadlocks!

// Correct: don't double lock

// BUG: Channel deadlock
ch := make(chan int)
ch <- 1  // Blocks forever! No receiver

// FIX: Buffered channel or goroutine
ch := make(chan int, 1)
ch <- 1

// OR
go func() {
    ch <- 1
}()

// BUG: Circular wait
// Goroutine 1:
mu1.Lock()
mu2.Lock()  // Waiting for mu2

// Goroutine 2:
mu2.Lock()
mu1.Lock()  // Waiting for mu1

// FIX: Consistent lock ordering
func safeOperation() {
    mu1.Lock()
    defer mu1.Unlock()
    mu2.Lock()
    defer mu2.Unlock()
    // Always acquire mu1 before mu2
}

// BUG: Forgotten send/recv
ch := make(chan int)
value := <-ch  // Blocks forever!

// FIX: Use select with default
select {
case value := <-ch:
    // Use value
case <-time.After(time.Second):
    return ErrTimeout
default:
    return ErrNoData
}

// BUG: Waiting on own channel
func process() {
    ch := make(chan int)
    // Forgot to start receiver
    <-ch  // Deadlock!
}

// FIX: Always ensure receiver exists
func process() {
    ch := make(chan int)
    go func() {
        ch <- compute()
    }()
    result := <-ch
}
```

**Detection Tools:**
```go
// Enable deadlock detector in tests
import "deadlock"

var mu = deadlock.Mutex{}  // Instead of sync.Mutex

// Will panic on deadlock instead of hanging
```

### 5. Goroutine Leaks

**Symptoms:**
- Goroutine count grows indefinitely
- Memory usage increases
- `runtime.NumGoroutine()` shows growth

**Detection:**
```bash
# Monitor goroutines
curl http://localhost:6060/debug/pprof/goroutine?debug=2

# In code
fmt.Println("Goroutines:", runtime.NumGoroutine())
```

**Common Patterns:**

```go
// BUG: Goroutine never exits
func forever() {
    go func() {
        for {
            time.Sleep(time.Second)
        }  // Never exits!
    }()
}

// FIX: Add context for cancellation
func forever(ctx context.Context) {
    go func() {
        ticker := time.NewTicker(time.Second)
        defer ticker.Stop()
        for {
            select {
            case <-ticker.C:
                // Do work
            case <-ctx.Done():
                return  // Exit!
            }
        }
    }()
}

// BUG: Blocked channel send (no receiver)
func leak() {
    ch := make(chan int)
    go func() {
        ch <- 42  // Goroutine stuck here
    }()
    // ch is never received from
}

// FIX: Ensure all sends are received
func noLeak() {
    ch := make(chan int)
    go func() {
        ch <- 42
    }()
    result := <-ch  // Receive the value
    fmt.Println(result)
}

// BUG: Abandoned goroutine on error
func process() error {
    ch := make(chan int)
    go func() {
        result := expensiveOp()
        ch <- result
    }()

    if someCondition {
        return ErrInvalid  // Goroutine leaked!
    }

    return <-ch
}

// FIX: Use context for cleanup
func process(ctx context.Context) error {
    ch := make(chan int)
    done := make(chan struct{})

    go func() {
        result := expensiveOpWithContext(ctx)
        select {
        case ch <- result:
        case <-done:
            return
        }
    }()

    if someCondition {
        close(done)  // Signal goroutine to exit
        return ErrInvalid
    }

    return <-ch
}

// BUG: Unbounded goroutine creation
func handleRequests() {
    for req := range requests {
        go process(req)  // Creates unlimited goroutines!
    }
}

// FIX: Worker pool pattern
func handleRequests() {
    sem := make(chan struct{}, 100)  // Max 100 workers
    for req := range requests {
        sem <- struct{}{}  // Acquire
        go func(req Request) {
            defer func() { <-sem }()  // Release
            process(req)
        }(req)
    }
}

// BUG: Forgotten timer
func timeout() {
    timer := time.NewTimer(time.Minute)
    if someCondition {
        return  // Timer never stopped!
    }
    <-timer.C
}

// FIX: Always stop timers
func timeout() {
    timer := time.NewTimer(time.Minute)
    defer timer.Stop()

    if someCondition {
        return
    }
    <-timer.C
}
```

**Detection Tools:**
```bash
# Goroutine leak detection
go test -run TestLeak -timeout 30s

# Monitor in production
import (
    "expvar"
    "runtime"
)

func init() {
    expvar.Publish("goroutines", expvar.Func(func() interface{} {
        return runtime.NumGoroutine()
    }))
}
```

### 6. Memory Leaks

**Symptoms:**
- Memory usage grows over time
- GC pressure increases
- OOM kills

**Detection:**
```bash
# Memory profiling
go test -memprofile=mem.prof ./...
go tool pprof -http=:8080 mem.prof

# Heap profiling
curl http://localhost:6060/debug/pprof/heap > heap.prof
go tool pprof -http=:8080 heap.prof

# Allocation profiling
go test -alloc_profile=alloc.prof
```

**Common Patterns:**

```go
// BUG: Goroutine holding reference
func leak() {
    var data []byte
    go func() {
        // Goroutine keeps data alive forever
        time.Sleep(time.Hour)
        _ = data
    }()
    data = nil  // But goroutine still has reference
}

// FIX: Explicitly release before goroutine
func noLeak() {
    var data []byte
    data = processData(data)
    go func() {
        // Only keep what's needed
        necessary := extractNecessary(data)
        time.Sleep(time.Hour)
        _ = necessary
    }()
    data = nil
}

// BUG: Slice growing forever
var cache []Item

func addItem(item Item) {
    cache = append(cache, item)  // Never cleared!
}

// FIX: Implement cache eviction
type Cache struct {
    items []Item
    max   int
}

func (c *Cache) Add(item Item) {
    c.items = append(c.items, item)
    if len(c.items) > c.max {
        // Remove oldest
        c.items = c.items[1:]
    }
}

// BUG: Unclosed resources
func process() error {
    f, _ := os.Open("file.txt")
    // If error or early return, file never closed
    data := make([]byte, 1024)
    f.Read(data)
    return nil
}

// FIX: Always defer close
func process() error {
    f, err := os.Open("file.txt")
    if err != nil {
        return err
    }
    defer f.Close()

    data := make([]byte, 1024)
    _, err = f.Read(data)
    return err
}

// BUG: String concatenation in loop
func buildString(items []string) string {
    var result string
    for _, item := range items {
        result += item  // Allocates new string each iteration!
    }
    return result
}

// FIX: Use strings.Builder
func buildString(items []string) string {
    var builder strings.Builder
    for _, item := range items {
        builder.WriteString(item)
    }
    return builder.String()
}

// BUG: Map growing unbounded
var cache = make(map[string]*Item)

func get(key string) *Item {
    if item, ok := cache[key]; ok {
        return item
    }
    item := &Item{...}
    cache[key] = item  // Never evicted!
    return item
}

// FIX: LRU cache or periodic cleanup
type Cache struct {
    data map[string]*Item
    max  int
}

func (c *Cache) Get(key string) *Item {
    item, ok := c.data[key]
    if !ok {
        item = &Item{...}
        if len(c.data) >= c.max {
            c.evict()
        }
        c.data[key] = item
    }
    return item
}
```

### 7. Context Misuse

**Common Patterns:**

```go
// BUG: Not passing context down
func handler(w http.ResponseWriter, r *http.Request) {
    data := fetchFromDB()  // Ignores request context!
}

// FIX: Pass context through
func handler(w http.ResponseWriter, r *http.Request) {
    data := fetchFromDB(r.Context())
}

func fetchFromDB(ctx context.Context) ([]Data, error) {
    // Use ctx in DB query
    db.QueryContext(ctx, "SELECT...")
}

// BUG: Ignoring context cancellation
func process(ctx context.Context) error {
    go func() {
        for {
            time.Sleep(time.Second)
            // Doesn't check ctx.Done()!
        }
    }()
    return nil
}

// FIX: Respect context
func process(ctx context.Context) error {
    go func() {
        ticker := time.NewTicker(time.Second)
        defer ticker.Stop()
        for {
            select {
            case <-ticker.C:
                // Do work
            case <-ctx.Done():
                return  // Exit!
            }
        }
    }()
    return nil
}

// BUG: Wrong context type
func doSomething(ctx context.Context) {
    deadline, ok := ctx.Deadline()
    if !ok {
        // Using context.Background() might not be right
    }
}

// FIX: Use appropriate context
func doSomethingWithTimeout(ctx context.Context) error {
    ctx, cancel := context.WithTimeout(ctx, 30*time.Second)
    defer cancel()
    return doSomething(ctx)
}

// BUG: Canceling parent context
func process(ctx context.Context) {
    ctx, cancel := context.WithCancel(ctx)
    // Calling cancel() cancels the parent!
}

// FIX: Only cancel derived contexts
func process(ctx context.Context) {
    child, cancel := context.WithCancel(ctx)
    defer cancel()  // Only cancels child
    // Use child
}
```

### 8. Error Handling Bugs

**Common Patterns:**

```go
// BUG: Ignoring errors
file, _ := os.Open("file.txt")  // Error ignored!
data, _ := ioutil.ReadAll(file)

// FIX: Always handle errors
file, err := os.Open("file.txt")
if err != nil {
    return fmt.Errorf("open file: %w", err)
}

// BUG: Losing error context
return fmt.Errorf("operation failed: %v", err)

// FIX: Use error wrapping
return fmt.Errorf("operation failed: %w", err)

// BUG: Creating errors as strings
return fmt.Errorf("invalid input: " + input)

// FIX: Use error variables or types
var ErrInvalidInput = errors.New("invalid input")
return ErrInvalidInput

// BUG: Sentinel errors with wrapping
if errors.Is(err, ErrNotFound) {
    // This won't work if err was wrapped!
}

// FIX: Use errors.Is correctly
if errors.Is(err, ErrNotFound) {
    // Works even with wrapped errors
}

// BUG: Error type assertions
type ValidationError struct {
    Field string
}

if e, ok := err.(*ValidationError); ok {
    // Won't work if error was wrapped!
}

// FIX: Use errors.As
var ve *ValidationError
if errors.As(err, &ve) {
    fmt.Println("Field:", ve.Field)
}
```

### 9. Interface and Type Assertion Bugs

**Common Patterns:**

```go
// BUG: Panic on type assertion
var i interface{} = "hello"
n := i.(int)  // Panics!

// FIX: Use comma-ok
n, ok := i.(int)
if !ok {
    return fmt.Errorf("not an int")
}

// BUG: Nil interface with value
func returnsError() error {
    var p *MyError = nil
    return p  // Returns non-nil interface with nil value!
}

func check() error {
    err := returnsError()
    if err != nil {  // Always true!
        return err
    }
    return nil
}

// FIX: Return concrete nil
func returnsError() error {
    return nil  // Not a typed nil
}

// BUG: Pointer vs value receivers
type Counter struct {
    value int
}

func (c Counter) Increment() {
    c.value++  // Modifies copy!
}

// FIX: Use pointer for mutation
func (c *Counter) Increment() {
    c.value++
}

// BUG: Copying sync values
func process(s struct {
    mu sync.Mutex
}) {
    s2 := s  // Copying mutex is bad!
    s2.mu.Lock()
}

// FIX: Never copy sync values
func process(s *struct {
    mu sync.Mutex
}) {
    s.mu.Lock()
}
```

## Debugging Tools & Techniques

### 1. Logging

```go
// Use structured logging
import "log/slog"

func process(id string) {
    slog.Debug("starting process",
        "id", id,
        "timestamp", time.Now(),
    )

    result, err := doWork(id)
    if err != nil {
        slog.Error("work failed",
            "id", id,
            "error", err,
        )
        return err
    }

    slog.Info("process completed",
        "id", id,
        "result", result,
    )
}

// Conditional logging
func debugLog(format string, args ...interface{}) {
    if debugMode {
        log.Printf(format, args...)
    }
}

// Contextual logging
func withContext(ctx context.Context, msg string) {
    log.Printf("%s: trace_id=%s", msg, ctx.Value("trace_id"))
}
```

### 2. Delve Debugger

```bash
# Install
go install github.com/go-delve/delve/cmd/dlv@latest

# Debug a program
dlv debug ./cmd/myapp

# Common commands
(dlv) break main.main      # Set breakpoint
(dlv) breakpoints          # List breakpoints
(dlv) next                 # Next line
(dlv) step                 # Step into function
(dlv) stepout              # Step out
(dlv) continue             # Continue execution
(dlv) print variable       # Print variable
(dlv) locals               # Show local variables
(dlv) args                 # Show function arguments
(dlv) goroutines           # List goroutines
(dlv) goroutine 5          # Switch to goroutine 5
(dlv) stack                # Show stack trace

# Debug test
dlv test ./pkg

# Debug running process
dlv attach <pid>
```

### 3. Runtime Metrics

```go
// Enable pprof HTTP server
import _ "net/http/pprof"

func main() {
    go func() {
        log.Println(http.ListenAndServe("localhost:6060", nil))
    }()

    // Application code
}

// Access profiles
// http://localhost:6060/debug/pprof/
# - goroutine    - Goroutine stacks
# - heap         - Memory allocation
# - threadcreate - OS thread creation
# - block        - Blocking operations
# - mutex        - Mutex contention
# - profile      - CPU profile (seconds=30)
# - trace        - Execution trace (seconds=5)
```

### 4. Execution Tracing

```bash
# Collect trace
go test -trace=trace.out ./...

# Analyze trace
go tool trace trace.out

# View in browser
# Opens with interactive trace viewer
```

```go
// Add custom trace events
import "runtime/trace"

func main() {
    trace.Start(os.Stdout)
    defer trace.Stop()

    // Application code
}

// Add task tracing
ctx, task := trace.NewTask(ctx, "processRequest")
defer task.End()

trace.WithRegion(ctx, "database", func() {
    // Database operation
})
```

## Panic Recovery

```go
// Top-level recovery
func main() {
    defer func() {
        if r := recover(); r != nil {
            log.Printf("PANIC: %v\n%s", r, debug.Stack())
            // Cleanup
        }
    }()

    runApplication()
}

// Per-goroutine recovery
func safeGoroutine() {
    defer func() {
        if r := recover(); r != nil {
            log.Printf("Goroutine panic: %v\n%s", r, debug.Stack())
        }
    }()

    // Goroutine work
}

// HTTP handler recovery
func panicRecovery(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        defer func() {
            if err := recover(); err != nil {
                http.Error(w, "Internal Server Error", 500)
                log.Printf("Panic: %v\n%s", err, debug.Stack())
            }
        }()
        next.ServeHTTP(w, r)
    })
}
```

## Testing Strategies

```go
// Table-driven tests for bug scenarios
func TestEdgeCases(t *testing.T) {
    tests := []struct {
        name    string
        input   string
        wantErr bool
    }{
        {"empty input", "", true},
        {"nil pointer trigger", "nil", true},
        {"overflow case", string([]byte{0xff, 0xff, 0xff, 0xff}), true},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            err := Process(tt.input)
            if (err != nil) != tt.wantErr {
                t.Errorf("Process() error = %v, wantErr %v", err, tt.wantErr)
            }
        })
    }
}

// Race detection tests
func TestConcurrentAccess(t *testing.T) {
    var counter Counter

    var wg sync.WaitGroup
    for i := 0; i < 100; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            counter.Increment()
        }()
    }
    wg.Wait()

    if counter.Value() != 100 {
        t.Errorf("counter = %d; want 100", counter.Value())
    }
}

// Run with: go test -race
```

## Bug Fixing Checklist

Before declaring a bug fixed, verify:

- [ ] Root cause is understood, not just symptoms
- [ ] Fix addresses the root cause
- [ ] Tests added for the bug scenario
- [ ] All existing tests still pass
- [ ] Race detector shows no issues (`go test -race`)
- [ ] No new lint warnings (`staticcheck ./...`)
- [ ] No new vulnerabilities (`go list -json -m all | nancy sleuth`)
- [ ] Performance impact measured (`go test -bench`)
- [ ] Documentation updated if behavior changed
- [ ] Similar code locations checked for same issue

## Prevention Strategies

### Code Review Checklist

- [ ] All errors checked
- [ ] No goroutines or channels in loops without bounds
- [ ] Shared state properly synchronized
- [ ] Resources closed with defer
- [ ] Context passed through call chain
- [ ] No nil pointer assumptions
- [ ] Slice bounds checked
- [ ] Type assertions use comma-ok
- [ ] Error wrapping used appropriately
- [ ] No unchecked type conversions

### Static Analysis

```bash
# go vet
go vet ./...

# staticcheck
go install honnef.co/go/tools/cmd/staticcheck@latest
staticcheck ./...

# golangci-lint
go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest
golangci-lint run

# errcheck (find unchecked errors)
go install github.com/kisielk/errcheck@latest
errcheck ./...

# nilaway (nil pointer detection)
go install go.uber.org/nilaway/cmd/nilaway@latest
nilaway ./...
```

### CI/CD Integration

```yaml
# GitHub Actions
name: Test
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-go@v4
        with:
          go-version: '1.21'

      - name: Race detector
        run: go test -race ./...

      - name: Vet
        run: go vet ./...

      - name: Staticcheck
        run: staticcheck ./...

      - name: Coverage
        run: go test -coverprofile=coverage.out ./...

      - name: Upload coverage
        uses: codecov/codecov-action@v3
```

## Debugging Workflow

1. **Reproduce the bug** - Create minimal reproduction
2. **Add logging** - Instrument suspected code
3. **Use tools** - Race detector, pprof, delve
4. **Isolate cause** - Binary search or bisect
5. **Write test** - Capture bug in test case
6. **Implement fix** - Address root cause
7. **Verify fix** - Test passes, no regressions
8. **Add prevention** - Lint rules, documentation

## When to Use This Skill

Invoke this skill when:
- Investigating crashes or panics
- Debugging race conditions
- Finding memory leaks
- Analyzing performance problems
- Fixing deadlocks or livelocks
- Troubleshooting flaky tests
- Resolving resource leaks
- Fixing concurrency bugs
- Understanding unexpected behavior

This skill provides systematic debugging methodology covering all common Go bug patterns with proven solutions and prevention strategies.
