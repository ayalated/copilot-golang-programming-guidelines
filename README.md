<p align="center">
  <a href="./README.zh_CN.md">简体中文</a> |
  <a href="./README.zh_TW.md">繁體中文</a> |
  <strong>English</strong> |
  <a href="./README.fr.md">Français</a> |
  <a href="./README.ja.md">日本語</a>
</p>

# Go Coding & Design Rules (for AI / Copilot)

> All generated code must follow these rules.

---

# 1. Core Philosophy

* Follow **Unix philosophy**
* Follow **Go idioms**
* Prefer **simple, explicit, composable design**

---

# 2. Unix-style Design (MANDATORY)

* Do one thing well
* Prefer small, focused functions
* Prefer composition over large abstractions
* Design for composability

Preferred:

```go
func Process(r io.Reader, w io.Writer) error
```

Avoid:

```go
func ProcessFile(path string) ([]byte, error)
```

---

# 3. Interfaces

* Keep interfaces **small (1–2 methods)**
* Define interfaces at **usage side**
* Use `-er` naming for single-method interfaces

Good:

```go
type Store interface {
    Save(User) error
}
```

Bad:

```go
type Service interface {
    Create()
    Update()
    Delete()
    Send()
}
```

---

# 4. Error Handling

* Always handle errors explicitly
* Never ignore errors
* Wrap errors with context

```go
if err != nil {
    return fmt.Errorf("create user: %w", err)
}
```

---

# 5. Functions

* Must be small and focused
* Prefer ≤ 50 lines
* Prefer ≤ 3 parameters
* One responsibility per function

---

# 6. Struct Design

* Avoid "God struct"
* Use composition
* Avoid `interface{}` unless necessary

Bad:

```go
type App struct {
    DB
    Redis
    Logger
}
```

---

# 7. Data Flow

* Prefer `io.Reader` / `io.Writer`
* Avoid hardcoded file paths or global state

---

# 8. Concurrency

* Do not start goroutines without control
* All goroutines must be cancelable
* Always use `context.Context`

```go
func Run(ctx context.Context)
```

---

# 9. Channels

* Default: unbuffered or size 1
* Avoid complex channel orchestration

---

# 10. Time Handling

* Use `time.Time` and `time.Duration`
* Never use `int` for time

---

# 11. init()

* Avoid `init()` unless absolutely necessary

---

# 12. main()

* Keep `main()` minimal
* Do not put business logic in `main`

---

# 13. Logging

* Use structured logging (e.g. slog)
* Do not use `fmt.Println`

---

# 14. JSON / API

* Use explicit struct fields
* Avoid `map[string]interface{}`

---

# 15. Data Safety (Uber practice)

* Do not expose internal mutable slices/maps
* Copy at boundaries when needed

```go
return append([]T(nil), data...)
```

---

# 16. General Rules

* No global mutable state
* No hidden side effects
* Prefer explicit over implicit
* Prefer readability over cleverness

---

# 17. Copilot Instructions (IMPORTANT)

When generating code, always:

* Use small interfaces
* Avoid large abstractions
* Avoid unnecessary layers
* Avoid unused code
* Avoid over-engineering
* Prefer idiomatic Go
* Prefer composable design

---

# 18. Guiding Principle

> Write code that is easy to compose, test, and reason about.
