---
name: go-analyzer
description: Comprehensive Go project analysis. Deep dive into Go codebases covering architecture, dependencies, concurrency patterns, interfaces, performance, testing, and best practices. Use this skill when you need thorough analysis of Go projects.
license: MIT
---

This skill provides comprehensive, Go-specific analysis of Go projects and codebases. It goes beyond general repository analysis to cover Go's unique features, patterns, and best practices.

The user invokes this skill by asking to analyze a Go project, repository, or codebase.

## Comprehensive Analysis Framework

### Phase 1: Project Structure & Module Analysis

**Identify Go project layout:**
- Use Glob to find `go.mod`, `go.sum`, `*.go` files
- Determine project type:
  - **Standard library layout**: cmd/, pkg/, internal/
  - **Microservice**: cmd/service/, api/, internal/
  - **Library**: package root, examples/, go.mod
  - **CLI tool**: cmd/, internal/
  - **Plugin/OpenTelemetry collector**: components/

**Analyze go.mod:**
- Module path and version
- Go version requirement
- Direct dependencies (not indirect)
- Replace and exclude directives
- Dependency versions and potential updates

**Map directory structure:**
- `cmd/` - Entry points and main packages
- `pkg/` - Public libraries for external use
- `internal/` - Private code (not importable by external modules)
- `api/` - API definitions (OpenAPI, protobuf, gRPC)
- `configs/` - Configuration files
- `scripts/` - Build, install, analysis scripts
- `test/` - Additional test data and utilities
- `docs/` - Documentation
- `examples/` - Example usage
- `third_party/` - Third-party dependencies
- `tools/` - Supporting tools
- `web/` - Web application assets
- `deployments/` - Deployment configs (docker, k8s)
- `.github/` - GitHub Actions workflows

### Phase 2: Package & Dependency Graph

**Analyze package organization:**
- Use Grep to find all `package` declarations
- Map package dependencies and import relationships
- Identify circular dependencies (anti-pattern)
- Check for package naming conventions:
  - Lowercase, single word when possible
  - Descriptive but concise
  - No underscores or mixedCaps

**Import analysis:**
- Standard library vs external dependencies
- Relative imports (should not exist in modules)
- Grouped imports (gofmt standard: std, third-party, local)
- Identify unused imports
- Check for import "." usage (generally discouraged)

**Dependency health:**
- Use `go mod graph` patterns to understand dependency tree
- Identify transitive dependencies
- Note any deprecated or unmaintained packages
- Check for duplicate functionality across packages

### Phase 3: Interface & Type System Analysis

**Interface discovery (Grep pattern: `type.*interface`):**
- Identify all interface definitions
- Find interface implementations (implicit satisfaction)
- Check for empty interfaces (`interface{}` or `any`)
- Look for one-method interfaces (excellent naming opportunity)
- Identify interface composition

**Type definitions (Grep pattern: `type.*struct`, `type.*alias`):**
- All structs and their fields
- Field tags (JSON, YAML, DB, xml, etc.)
- Type aliases vs type definitions
- Embedded types (composition vs inheritance)
- Generic types (Go 1.18+)

**Design patterns:**
- Strategy pattern (interfaces)
- Decorator pattern (embedded structs)
- Facade pattern (simplified APIs)
- Builder pattern (functional options)
- Singleton pattern (sync.Once)
- Observer pattern (channels)

### Phase 4: Concurrency & Parallelism

**Goroutine analysis (Grep pattern: `go\s+`):**
- Identify all goroutine creation points
- Check for goroutine leaks (no exit condition)
- Find unbounded goroutine creation
- Identify worker pool patterns

**Channel usage (Grep pattern: `make\(chan`, `<-`, `chan\s+(int|struct|bool)`):**
- Buffered vs unbuffered channels
- Channel direction (send-only, receive-only, bidirectional)
- Detect potential deadlocks
- Find unclosed channels
- Identify select statements with default clauses (non-blocking)

**Concurrency patterns:**
- **Worker pool**: Fixed goroutines processing work from channel
- **Pipeline**: Series of stages connected by channels
- **Fan-out/Fan-in**: Distributing work, collecting results
- **Context cancellation**: Proper propagation
- **sync usage**: Mutex, RWMutex, WaitGroup, Once, Map, Cond, Pool
- **atomic usage**: Atomic operations for performance
- **sync/errgroup**: Group goroutines with error propagation

**Common concurrency issues:**
- Data races (race detector findings)
- Goroutine leaks
- Channel misuse
- Missing mutex protection
- Copy of sync values (never copy!)

### Phase 5: Error Handling & Patterns

**Error handling analysis:**
- Use Grep to find `if.*err.*!=.*nil` patterns
- Check for ignored errors (blank identifier `_`)
- Identify custom error types (`type.*error`)
- Find error wrapping (`fmt.Errorf`, `errors.Wrap`)
- Check for sentinel errors (e.g., `io.EOF`)

**Error patterns:**
- Sentinel errors comparison (`errors.Is`)
- Error type assertions (`errors.As`)
- Error wrapping for context (`%w` verb)
- Panic/recover usage (should be rare)
- Defers with error handling

**Best practices check:**
- Errors are not strings (avoid `errors.New("...: " + msg)`)
- Early returns (guard clauses)
- Error variables (package-level)
- Custom error types with methods

### Phase 6: Testing & Quality

**Test coverage (Glob: `*_test.go`):**
- Find all test files
- Test package structure (same package vs package_test)
- Table-driven tests
- Test helpers and fixtures
- Subtest usage (`t.Run()`)

**Test types:**
- Unit tests (`*_test.go`)
- Integration tests (often in `test/` or `integration/`)
- Benchmark tests (`Benchmark*` functions)
- Example tests (`Example*` functions with Output comments)
- Fuzzing tests (`Fuzz*` functions, Go 1.18+)

**Testing patterns:**
- Test setup/teardown (TestMain, t.Cleanup)
- Mock usage (interfaces, gomock, testify/mock)
- Test coverage analysis (`go test -cover`)
- Race detection in tests (`go test -race`)

**Code quality indicators:**
- `go vet` patterns
- `staticcheck` findings
- `golangci-lint` configuration
- Effective Go guidelines adherence
- Comment godoc coverage (`// FunctionName ...`)
- Exported identifiers have comments

### Phase 7: Performance & Optimization

**Performance patterns:**
- String building (strings.Builder vs +)
- Slice pre-allocation (make with capacity)
- Map pre-allocation
- Avoid allocations in hot paths
- sync.Pool usage
- Benchmark comparison

**Memory management:**
- Escape analysis (stack vs heap allocation)
- Slice vs array usage
- Pointer vs value semantics
- GC pressure indicators

**Profiling points:**
- CPU profiling hotspots
- Memory allocation patterns
- Goroutine blocking
- System calls

### Phase 8: Configuration & Build

**Build tags (Grep: `//go:build` or `// +build`):**
- Platform-specific code
- Feature flags
- Custom builds

**Linkage and embedding:**
- Static analysis
- Embed directive (`//go:embed`)
- CGO usage
- Assembly files (`*.s`, `*.asm`)

**Tooling:**
- Makefiles and build scripts
- Go generate usage (`//go:generate`)
- Linting configuration (golangci-lint)
- CI/CD integration (GitHub Actions, GitLab CI)

### Phase 9: Security & Best Practices

**Security considerations:**
- Input validation
- SQL injection (prepared statements)
- XSS protection (html/template vs template)
- Path traversal (filepath.Clean)
- Crypto usage (proper randomness, key management)
- Dependency vulnerabilities (`go list -json -m all`)

**Go idioms check:**
- Error handling first (not if err == nil)
- Receiver naming (consistent, one or two letters)
- Exported identifiers (comments, godoc)
- Package comments
- Zero value usability
- Interface definitions (small, focused)
- Context propagation (first parameter)
- Pointer receivers (consistency)

**Anti-patterns detection:**
- `init()` functions (avoid when possible)
- Global mutable state
- Package-level variables without good reason
- Deeply nested code
- Magic numbers and strings
- Overly generic names (data, info, util)

### Phase 10: API & Protocol Analysis

**API definitions:**
- REST API handlers (net/http, gorilla/mux, gin, echo, chi)
- gRPC service definitions
- GraphQL schemas
- WebSocket handlers

**Protocol buffers:**
- `.proto` files in `proto/` or `api/`
- Generated code location
- gRPC server/client implementations

**OpenAPI/Swagger:**
- OpenAPI specifications
- Code generation from specs

## Output Format

Provide a comprehensive analysis report:

```
## Go Project Analysis: [project-name]

### Project Overview
- **Module**: [module-path]
- **Go Version**: [version]
- **Project Type**: [library/service/cli/plugin]
- **Purpose**: [brief description]

### Repository Structure
```
[Directory tree with descriptions]
```

### Module & Dependencies
- **Direct Dependencies**: [count]
- **Indirect Dependencies**: [count]
- **Notable Dependencies**: [list major deps]
- **Dependency Health**: [any concerns]

### Package Architecture
- **Total Packages**: [count]
- **Entry Points**: [list cmd/ packages]
- **Key Packages**: [describe important packages]
- **Import Graph**: [notable relationships]
- **Circular Dependencies**: [yes/no + details]

### Interface Design
- **Total Interfaces**: [count]
- **Key Interfaces**: [list important ones]
- **Interface Composition**: [examples]
- **Design Patterns**: [identified patterns]

### Concurrency Model
- **Goroutine Usage**: [usage patterns]
- **Channel Patterns**: [buffered/unbuffered, select usage]
- **Sync Primitives**: [mutex, waitgroup, etc.]
- **Concurrency Safety**: [race detector findings, concerns]
- **Identified Patterns**: [worker pool, pipeline, etc.]

### Error Handling
- **Error Style**: [sentinel/wrapping/custom types]
- **Panic/Recover**: [usage]
- **Error Propagation**: [context preservation]
- **Best Practices**: [adherence level]

### Testing Strategy
- **Test Coverage**: [files, packages]
- **Test Types**: [unit/integration/benchmark/fuzz]
- **Test Patterns**: [table-driven, mocks]
- **CI Integration**: [yes/no]

### Performance Considerations
- **Optimization Opportunities**: [areas for improvement]
- **Memory Patterns**: [allocation hotspots]
- **Profiling**: [recommendations]
- **Benchmarks**: [presence]

### Build & Tooling
- **Build System**: [make/just/go build]
- **Code Generation**: [go generate usage]
- **Linting**: [tools and rules]
- **CI/CD**: [pipeline status]

### Security Assessment
- **Vulnerabilities**: [known issues]
- **Security Best Practices**: [adherence]
- **Input Validation**: [coverage]
- **Cryptography**: [usage and safety]

### Code Quality Summary
- **Idiomatic Go**: [rating]
- **Documentation**: [godoc coverage]
- **Anti-patterns**: [identified issues]
- **Technical Debt**: [areas needing attention]

### Key Files & Navigation
- **Entry Points**: [file paths]
- **Core Logic**: [file paths]
- **Configuration**: [file paths]
- **Tests**: [key test files]

### Recommendations
1. [Prioritized improvement suggestions]
2. [Best practice adoption opportunities]
3. [Refactoring candidates]
4. [Documentation needs]

### Quick Start Commands
```bash
go run ./cmd/[service]      # Run the service
go test ./...               # Run all tests
go test -race ./...         # Run with race detector
go vet ./...                # Run go vet
go build ./...              # Build all packages
```
```

## Analysis Commands to Use

Prefer these patterns during analysis:

```bash
# Go structure
find . -name "go.mod" -o -name "*.go"
go list -m all                          # All dependencies
go list -json ./...                     # Package information

# Code patterns
rg "go\s+\w+\("                          # Goroutine creation
rg "make\(chan"                          # Channel creation
rg "type.*interface"                     # Interface definitions
rg "func.*\).*error"                     # Error returning functions
rg "//go:generate"                       # Code generation
rg "//go:embed"                          # Embedded files

# Build and test
go build ./...
go test ./... -cover
go test ./... -race
go vet ./...
```

## Special Considerations

- **Go versions**: Features differ between 1.18+ (generics), 1.19+ (improved tooling), 1.21+ (slog, slices/maps), 1.22+ (range over int, for loop changes), 1.23+ (toolchain, telemetry)
- **Standard library**: Prefer stdlib over external deps when possible
- **API stability**: Check for module versioning and semantic import versioning
- **Breaking changes**: Look for major version upgrades (/v2, /v3)
- **Deprecated code**: Check for Deprecated comments
- **Vendor directory**: Note if vendoring is used

This skill provides deep, Go-aware analysis that considers Go's unique design philosophy, tooling ecosystem, and best practices.
