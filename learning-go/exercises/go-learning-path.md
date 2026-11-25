# Go Learning Path: 20 Exercises (Learning Go, 2nd ed.)

These 20 exercises follow the chapter order of _Learning Go_ (2nd ed.). Each one builds on the ones before it, and they're aimed at data pipelines, back-end services and AI agent tooling. Tick each one off as you finish it.

**Tip:** Write a quick Python version of each exercise first, then do it in Go and note what felt different. That comparison helps you start thinking the way Go code is written instead of writing Python in Go syntax.

## Part 1: Basics (Ch. 1–4)

- [ ] **1. Toolchain warm-up: `wc` clone.** Build a CLI that counts the lines, words and bytes in a file passed as an argument. Use `go mod init`, `go build`, `go vet` and `gofmt`, and add a Makefile. _Focus:_ project layout, `os.Args`, `bufio.Scanner`.
- [ ] **2. Unit converter with typed constants.** Convert bytes to KB/MB/GB/TB using `const` and `iota`. Then try mixing `int`, `int64` and `float64` and see where the compiler stops you. _Focus:_ Go has no implicit numeric conversion, unlike Python.
- [ ] **3. Word frequency counter.** Read a text file, normalize the words, count them in a `map[string]int` and print the top N. Sorting requires turning the map into a slice of structs. _Focus:_ maps, slices, `slices.SortFunc`.
- [ ] **4. Slice-aliasing puzzles.** Write small programs that show `append` changing a shared underlying array, the difference between `len` and `cap`, and how full slice expressions (`s[a:b:c]`) prevent it. Predict each output before you run it. _Focus:_ the slice gotcha every Python developer runs into.
- [ ] **5. FizzBuzz++ and a shadowing hunt.** Do FizzBuzz with a `switch` that has no condition. Then deliberately shadow a variable inside an `if` block and catch it with `go vet` or `shadow`. _Focus:_ blocks, scope, `for` in all four forms.

## Part 2: Functions, Pointers, Types (Ch. 5–7)

- [ ] **6. Pipeline of function transforms.** Write `Map`, `Filter` and `Reduce` over `[]int` using function values and closures. Build a `counter()` closure that holds state. Use `defer` to log how long each function takes. _Focus:_ first-class functions, closures, `defer`.
- [ ] **7. Pointer vs. value benchmarks.** Write a function that modifies a large struct, once by value and once by pointer. Measure both with `testing.B` and explain the results. _Focus:_ when pointers matter, and when they're just noise.
- [ ] **8. CSV record parser with methods.** Parse a CSV of clinical-trial-style records into `[]Record`. Add methods like `(r Record) IsActive() bool` and `(r *Record) Normalize()`. _Focus:_ value vs. pointer receivers, `encoding/csv`, struct tags.
- [ ] **9. Pluggable storage interface.** Define `type Store interface { Get(id string) (Record, error); Put(Record) error }`. Implement `MemoryStore` and `FileStore` (JSON on disk), and write code that only depends on the interface. _Focus:_ implicit interfaces, "accept interfaces, return structs".
- [ ] **10. Embedding and a mini middleware chain.** Wrap a `Store` with a `LoggingStore` that embeds the base store and overrides one method. Then build a `Handler` func type with `Chain(middlewares...)`. _Focus:_ composition over inheritance, func types as interfaces.

## Part 3: Generics, Errors, Modules (Ch. 8–10)

- [ ] **11. Generic collections.** Write `Map[T, U any]`, `Filter[T any]`, a generic `Set[T comparable]` and a `Max[T cmp.Ordered]`. Rewrite exercise 6 using them. _Focus:_ type parameters, constraints, type inference.
- [ ] **12. Robust error handling for a fetcher.** Make a function that loads records and returns sentinel errors (`ErrNotFound`), a custom error type (`*ValidationError{Field, Reason}`) and wrapped errors (`fmt.Errorf("...: %w", err)`). The caller should use `errors.Is` and `errors.As`. _Focus:_ errors as values, which replace Python's `try/except` habits.
- [ ] **13. Multi-package module.** Split exercises 8–12 into `internal/store`, `internal/model` and `cmd/app`. Add a third-party dependency, use `go mod tidy`, and try `replace` with a local fork. _Focus:_ packages, visibility (capitalization), `internal/`, modules.

## Part 4: Tooling, Concurrency, stdlib (Ch. 11–13)

- [ ] **14. Lint and release setup.** Add `golangci-lint`, `govulncheck` and `go generate` (with `stringer` for an enum) to your project, and cross-compile binaries for Linux, macOS and Windows. _Focus:_ the Go tooling ecosystem.
- [ ] **15. Concurrent URL checker.** Take a list of URLs, check them with a worker pool of N goroutines, and collect results over channels. Then add a global timeout with `select` and `time.After`. _Focus:_ goroutines, buffered and unbuffered channels, `sync.WaitGroup`.
- [ ] **16. Fan-out/fan-in ETL pipeline.** Build three stages connected by channels: read lines → parse and validate → aggregate. Make sure every channel is closed properly, and run it with `-race` to prove there are no data races. Compare a `sync.Mutex` counter with a channel-based one. _Focus:_ pipeline patterns, the Go equivalent of Python ETL work.
- [ ] **17. REST API with only the stdlib.** Expose your `Store` over HTTP using `net/http` and the Go 1.22+ routing patterns (`GET /records/{id}`). Add JSON encoding, `log/slog` structured logging, and graceful shutdown. _Focus:_ back-end Go without a framework.

## Part 5: Context, Testing, and an Agent Capstone (Ch. 14–16)

- [ ] **18. Context-aware LLM client.** Write an HTTP client that calls an LLM API (or a mock server). Propagate `context.Context` for timeouts and cancellation, add retries with exponential backoff, and stream the response (SSE) through a channel. _Focus:_ `context`, which is essential for agent and tool-calling code.
- [ ] **19. Test suite for everything.** Write table-driven tests, use `httptest.Server` for exercises 17 and 18, add a fuzz test for the CSV parser, and get coverage above 80%. Add one benchmark. _Focus:_ `testing`, subtests, fuzzing, test doubles via interfaces.
- [ ] **20. Capstone: an MCP server in Go.** Build an MCP server over stdio that exposes 2–3 tools, such as `search_records` and `get_record` backed by your `Store`, plus `fetch_url` reusing exercise 15. You can implement the JSON-RPC 2.0 messages yourself first, then port it to the official Go MCP SDK (`github.com/modelcontextprotocol/go-sdk`). Connect it to Claude Desktop or an MCP inspector. _Focus:_ putting it all together: interfaces, generics, errors, concurrency, context, testing, and reflection/struct tags (Ch. 16) for generating tool input schemas.
