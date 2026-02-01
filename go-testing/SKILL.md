---
name: go-testing
description: Comprehensive Go unit testing guidance. Write, improve, and analyze Go tests including table-driven tests, mocking, coverage, benchmarks, fuzzing, and best practices. Use this skill when working with Go testing.
license: MIT
---

This skill provides expert guidance for Go unit testing, from writing new tests to improving existing test suites and debugging test issues.

The user invokes this skill by asking to write tests, improve tests, debug test failures, or analyze test coverage for Go code.

## Core Testing Philosophy

Go testing principles:
- **Table-driven tests**: Preferred approach for multiple test cases
- **Clear failure messages**: Tests should explain what went wrong
- **Test isolation**: Each test should be independent
- **Fast feedback**: Unit tests should be quick
- **Readable**: Tests should document expected behavior

## Test Writing Patterns

### 1. Table-Driven Tests (Primary Pattern)

The most common and recommended Go testing pattern:

```go
func TestAdd(t *testing.T) {
    tests := []struct {
        name     string
        a, b     int
        expected int
    }{
        {"positive numbers", 2, 3, 5},
        {"negative numbers", -2, -3, -5},
        {"mixed signs", -2, 3, 1},
        {"zeros", 0, 0, 0},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            result := Add(tt.a, tt.b)
            if result != tt.expected {
                t.Errorf("Add(%d, %d) = %d; want %d", tt.a, tt.b, result, tt.expected)
            }
        })
    }
}
```

### 2. Subtests for Organization

Use `t.Run()` to group related tests:

```go
func TestUserValidation(t *testing.T) {
    t.Run("email validation", func(t *testing.T) {
        // Email-specific tests
    })

    t.Run("password validation", func(t *testing.T) {
        // Password-specific tests
    })
}
```

### 3. Test Helpers and Setup

Create reusable test utilities:

```go
// In test package
func newTestUser(t *testing.T) *User {
    t.Helper()
    user, err := NewUser("test@example.com", "password123")
    if err != nil {
        t.Fatalf("failed to create test user: %v", err)
    }
    return user
}

func assertEqual[T comparable](t *testing.T, got, want T) {
    t.Helper()
    if got != want {
        t.Errorf("got %v, want %v", got, want)
    }
}
```

### 4. Setup and Teardown

```go
func TestMain(m *testing.M) {
    // Global setup
    setupTestDatabase()
    defer teardownTestDatabase()

    // Run tests
    code := m.Run()
    os.Exit(code)
}

// Or per-test cleanup
func TestSomething(t *testing.T) {
    defer cleanup() // Runs after test completes
    // Test code here
}
```

## Mocking Techniques

### 1. Interface-Based Mocking (Preferred)

Define interfaces for dependencies:

```go
// Production code
type UserRepository interface {
    GetByID(id int) (*User, error)
}

type UserService struct {
    repo UserRepository
}

// Test with mock
type MockUserRepository struct {
    users map[int]*User
}

func (m *MockUserRepository) GetByID(id int) (*User, error) {
    user, ok := m.users[id]
    if !ok {
        return nil, ErrNotFound
    }
    return user, nil
}

func TestUserService_GetUser(t *testing.T) {
    mock := &MockUserRepository{
        users: map[int]*User{1: {ID: 1, Name: "Test"}},
    }
    service := NewUserService(mock)

    user, err := service.GetUser(1)
    if err != nil {
        t.Fatalf("unexpected error: %v", err)
    }
    if user.Name != "Test" {
        t.Errorf("got name %s, want Test", user.Name)
    }
}
```

### 2. Using gomock

For complex mocking scenarios:

```go
//go:generate go run github.com/golang/mock/mockgen -destination mock_service.go -package test github.com/example/pkg UserRepository

func TestWithGomock(t *testing.T) {
    ctrl := gomock.NewController(t)
    defer ctrl.Finish()

    mockRepo := test.NewMockUserRepository(ctrl)
    mockRepo.EXPECT().GetByID(1).Return(&User{ID: 1}, nil)

    service := NewUserService(mockRepo)
    user, err := service.GetUser(1)

    if err != nil {
        t.Fatalf("unexpected error: %v", err)
    }
}
```

### 3. testify/mock

Alternative to gomock:

```go
import "github.com/stretchr/testify/mock"

type MockRepo struct {
    mock.Mock
}

func (m *MockRepo) GetByID(id int) (*User, error) {
    args := m.Called(id)
    if args.Get(0) == nil {
        return nil, args.Error(1)
    }
    return args.Get(0).(*User), args.Error(1)
}
```

### 4. Test-Only Methods

Add methods for tests only:

```go
// In production file
type Config struct {
    // ... production fields
}

// testConfig returns a config for testing
func testConfig(t *testing.T) *Config {
    t.Helper()
    cfg, err := LoadConfig("../testdata/config.yaml")
    if err != nil {
        t.Fatal(err)
    }
    return cfg
}
```

## Testing Specific Scenarios

### 1. Error Testing

```go
func TestErrorCases(t *testing.T) {
    tests := []struct {
        name    string
        input   string
        wantErr error
    }{
        {"empty input", "", ErrEmptyInput},
        {"invalid format", "invalid", ErrInvalidFormat},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            err := Process(tt.input)
            if !errors.Is(err, tt.wantErr) {
                t.Errorf("got error %v, want %v", err, tt.wantErr)
            }
        })
    }
}
```

### 2. Concurrent Testing

Use `t.Parallel()` for parallel tests:

```go
func TestParallel(t *testing.T) {
    tests := []struct {
        name string
        // ...
    }{
        // Test cases...
    }

    for _, tt := range tests {
        tt := tt // Capture range variable
        t.Run(tt.name, func(t *testing.T) {
            t.Parallel() // Run in parallel

            // Test code
        })
    }
}
```

Test concurrent code:

```go
func TestConcurrentAccess(t *testing.T) {
    t.Parallel()

    var counter int64
    var wg sync.WaitGroup

    for i := 0; i < 100; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            atomic.AddInt64(&counter, 1)
        }()
    }

    wg.Wait()
    if counter != 100 {
        t.Errorf("counter = %d; want 100", counter)
    }
}
```

### 3. HTTP Handler Testing

```go
func TestHandler(t *testing.T) {
    tests := []struct {
        name           string
        method         string
        path           string
        body           string
        expectedStatus int
        expectedBody   string
    }{
        {"success", "GET", "/user/1", "", http.StatusOK, `{"id":1}`},
        {"not found", "GET", "/user/999", "", http.StatusNotFound, ""},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            req := httptest.NewRequest(tt.method, tt.path, strings.NewReader(tt.body))
            w := httptest.NewRecorder()

            handler.ServeHTTP(w, req)

            if w.Code != tt.expectedStatus {
                t.Errorf("status = %d; want %d", w.Code, tt.expectedStatus)
            }

            if tt.expectedBody != "" && w.Body.String() != tt.expectedBody {
                t.Errorf("body = %s; want %s", w.Body.String(), tt.expectedBody)
            }
        })
    }
}
```

### 4. Database Testing

Use testcontainers or SQLite in-memory:

```go
func setupTestDB(t *testing.T) *sql.DB {
    t.Helper()

    db, err := sql.Open("sqlite3", ":memory:")
    if err != nil {
        t.Fatal(err)
    }

    // Run migrations
    _, err = db.Exec(migrationSQL)
    if err != nil {
        t.Fatal(err)
    }

    t.Cleanup(func() {
        db.Close()
    })

    return db
}
```

### 5. File System Testing

```go
func TestWithFiles(t *testing.T) {
    tmpDir := t.TempDir() // Automatically cleaned up

    testFile := filepath.Join(tmpDir, "test.txt")
    err := os.WriteFile(testFile, []byte("test data"), 0644)
    if err != nil {
        t.Fatal(err)
    }

    // Test code using testFile
}
```

## Benchmark Writing

```go
func BenchmarkFunction(b *testing.B) {
    setup()

    b.ResetTimer() // Reset timer after setup
    for i := 0; i < b.N; i++ {
        Function()
    }
}

// Benchmark with different inputs
func BenchmarkParseJSON(b *testing.B) {
    input := []byte(`{"key":"value"}`)

    b.Run("encoding/json", func(b *testing.B) {
        for i := 0; i < b.N; i++ {
            var result map[string]string
            json.Unmarshal(input, &result)
        }
    })

    b.Run("jsoniter", func(b *testing.B) {
        for i := 0; i < b.N; i++ {
            var result map[string]string
            jsoniter.Unmarshal(input, &result)
        }
    })
}
```

## Example Tests

```go
// Example tests serve as both tests and documentation
func ExampleAdd() {
    result := Add(2, 3)
    fmt.Println(result)
    // Output: 5
}

// Example with multiple outputs
func ExampleMultiOutput() {
    sum, diff := Calculate(10, 3)
    fmt.Printf("%d %d", sum, diff)
    // Output: 13 7
}
```

## Fuzzing (Go 1.18+)

```go
func FuzzParseInt(f *testing.F) {
    // Add seed corpus
    f.Add("123")
    f.Add("-456")
    f.Add("0")

    f.Fuzz(func(t *testing.T, input string) {
        result, err := strconv.Atoi(input)
        if err != nil {
            return // Invalid input is okay
        }

        // Property: round-trip conversion
        if strconv.Itoa(result) != input {
            t.Errorf("round-trip failed: %d -> %s", result, input)
        }
    })
}
```

## Test Coverage Analysis

### Generate coverage reports:

```bash
# Terminal output
go test ./... -cover

# HTML report
go test ./... -coverprofile=coverage.out
go tool cover -html=coverage.out

# Specific package coverage
go test -coverprofile=coverage.out ./pkg/...
go tool cover -func=coverage.out | grep total

# Exclude generated code
go test -coverprofile=coverage.out ./pkg/...
go tool cover -html=coverage.out -o coverage.html
```

### Coverage thresholds:

```go
// In test file
func TestCoverage(t *testing.T) {
    // This test is just to hit coverage requirements
    if testing.Short() {
        t.Skip("skipping coverage test in short mode")
    }
    // ...
}
```

## Test Organization

### Package structure options:

```
# Option 1: Same package
pkg/
  user.go
  user_test.go  // Has access to unexported identifiers

# Option 2: Test package (black-box)
pkg/
  user.go
  user_test.go  // package user_test - only exported access

# Option 3: Test package with exported helper
pkg/
  user.go
  export_test.go  // package user - exports for testing
  user_test.go    // package user_test - uses exported helpers
```

### Export test helpers:

```go
// export_test.go
package user

// For testing access to internal state
func SetTestClock(u *User, clock Clock) {
    u.clock = clock
}
```

## Race Condition Testing

```bash
# Run tests with race detector
go test -race ./...

# Check specific test
go test -race -run TestConcurrent ./...
```

Common race patterns to test:
- Concurrent map access
- Shared struct field access
- Channel send/receive races
- Slice concurrent modification

## Test Assertions

### Standard library only:

```go
if got, want := result, expected; got != want {
    t.Errorf("result = %v; want %v", got, want)
}

// Error checking
if err != nil {
    t.Fatalf("unexpected error: %v", err)
}

// With context
if diff := cmp.Diff(got, want); diff != "" {
    t.Errorf("mismatch (-got +want):\n%s", diff)
}
```

### Using testify/assert:

```go
import "github.com/stretchr/testify/assert"

func TestWithTestify(t *testing.T) {
    assert.Equal(t, expected, result)
    assert.NoError(t, err)
    assert.Contains(t, result, "substring")
    assert.Len(t, items, 5)
}
```

### Using testify/assert with generics:

```go
func assertEqual[T comparable](t *testing.T, got, want T) {
    t.Helper()
    if got != want {
        t.Errorf("got %v, want %v", got, want)
    }
}

func assertIn[T comparable](t *testing.T, slice []T, item T) {
    t.Helper()
    for _, s := range slice {
        if s == item {
            return
        }
    }
    t.Errorf("%v not found in slice", item)
}
```

## Test Data Management

### Using testdata directory:

```
pkg/
  parser.go
  parser_test.go
testdata/
  valid_input.json
  invalid_input.json
  large_file.txt
```

```go
func TestParseFile(t *testing.T) {
    data, err := os.ReadFile("testdata/valid_input.json")
    if err != nil {
        t.Fatal(err)
    }

    result, err := Parse(data)
    // ...
}
```

### Embedded test data (Go 1.16+):

```go
//go:embed testdata/*.json
var testFiles embed.FS

func TestWithEmbed(t *testing.T) {
    data, _ := testFiles.ReadFile("testdata/test.json")
    // ...
}
```

## Common Testing Anti-Patterns

### Avoid these patterns:

```go
// DON'T: Inconsistent test data
func TestBad1(t *testing.T) {
    user := &User{ID: 1, Name: "Alice", Email: "alice@example.com", Age: 30, /* ... many fields */}
    // Hard to maintain
}

// DO: Use table-driven with clear test cases
func TestGood1(t *testing.T) {
    tests := []struct {
        name string
        user *User
        want error
    }{
        {"valid user", &User{Name: "Alice"}, nil},
        {"missing name", &User{}, ErrMissingName},
    }
    // ...
}

// DON'T: Magic values
if result > 100 { /* ... */ }

// DO: Named constants
const MaxThreshold = 100
if result > MaxThreshold { /* ... */ }

// DON'T: Testing implementation details
func TestBad2(t *testing.T) {
    if user.internalField != "expected" { /* ... */ }
}

// DO: Test observable behavior
func TestGood2(t *testing.T) {
    output := user.Process()
    if output != "expected" { /* ... */ }
}

// DON'T: Sleep-based synchronization
time.Sleep(100 * millisecond)

// DO: Proper synchronization
wg.Wait()
<-done
```

## Debugging Tests

### Verbose mode:

```bash
go test -v ./...
```

### Run specific test:

```bash
go test -run TestAdd ./...
go test -run "TestUser/.*validation" ./...
```

### Short mode (skip long tests):

```bash
go test -short ./...
```

```go
if testing.Short() {
    t.Skip("skipping integration test in short mode")
}
```

### Count vs. cover:

```bash
go test -count=1 ./...  # Disable cache
```

## CI/CD Integration

### GitHub Actions example:

```yaml
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
      - name: Test
        run: |
          go test -race -coverprofile=coverage.out -covermode=atomic ./...
      - name: Upload coverage
        uses: codecov/codecov-action@v3
```

## When to Use This Skill

Invoke this skill when:
- Writing new Go tests from scratch
- Improving or refactoring existing tests
- Debugging test failures or flaky tests
- Increasing test coverage
- Writing benchmarks or performance tests
- Setting up testing infrastructure
- Implementing mocking strategies
- Testing concurrent code
- Writing integration tests
- Implementing fuzzing tests

## Best Practices Summary

1. **Table-driven tests** for multiple scenarios
2. **t.Helper()** in helper functions
3. **t.Parallel()** for independent tests
4. **t.Cleanup()** for resource cleanup
5. **Testify** for assertion libraries (optional but helpful)
6. ** Interfaces** for mocking dependencies
7. **Race detector** for concurrent code
8. **Coverage reports** for identifying gaps
9. **Subtests** for organizing complex tests
10. **Clear error messages** for easy debugging

This skill provides comprehensive guidance for all Go testing scenarios, from simple unit tests to complex integration testing strategies.
