# Go 编码与设计规则（用于 AI / Copilot）

> 所有生成的代码必须遵循以下规则。

---

# 1. 核心理念

* 遵循 **Unix 哲学**
* 遵循 **Go 语言习惯（idiomatic Go）**
* 优先：**简单、显式、可组合**

---

# 2. Unix 风格设计（强制）

* 每个函数只做一件事
* 函数应小且专注
* 优先组合，而不是复杂抽象
* 设计必须可组合

推荐：

```go
func Process(r io.Reader, w io.Writer) error
```

避免：

```go
func ProcessFile(path string) ([]byte, error)
```

---

# 3. 接口设计

* 接口必须小（1–2 个方法）
* 接口定义在使用方
* 单方法接口使用 `-er` 命名

推荐：

```go
type Store interface {
    Save(User) error
}
```

避免：

```go
type Service interface {
    Create()
    Update()
    Delete()
    Send()
}
```

---

# 4. 错误处理

* 必须显式处理错误
* 禁止忽略错误
* 必须添加上下文

```go
if err != nil {
    return fmt.Errorf("create user: %w", err)
}
```

---

# 5. 函数设计

* 必须短小、单一职责
* 建议 ≤ 50 行
* 参数建议 ≤ 3 个

---

# 6. 结构体设计

* 禁止 God Struct
* 使用组合
* 避免使用 `interface{}`

避免：

```go
type App struct {
    DB
    Redis
    Logger
}
```

---

# 7. 数据流设计

* 优先使用 `io.Reader` / `io.Writer`
* 避免硬编码路径
* 避免全局状态

---

# 8. 并发规范

* 不要随意启动 goroutine
* 所有 goroutine 必须可控制、可退出
* 必须使用 `context.Context`

```go
func Run(ctx context.Context)
```

---

# 9. Channel 使用

* 默认无缓冲或 size=1
* 避免复杂 channel 编排

---

# 10. 时间处理

* 使用 `time.Time` / `time.Duration`
* 禁止用 `int` 表示时间

---

# 11. init()

* 尽量避免使用 `init()`

---

# 12. main()

* main 函数必须保持最简
* 不得包含业务逻辑

---

# 13. 日志

* 使用结构化日志（如 slog）
* 禁止使用 `fmt.Println`

---

# 14. JSON / API

* 使用明确结构体字段
* 避免 `map[string]interface{}`

---

# 15. 数据安全（Uber 实践）

* 不要暴露内部可变数据
* 必要时在边界复制数据

```go
return append([]T(nil), data...)
```

---

# 16. 通用规则

* 禁止全局可变状态
* 禁止隐式副作用
* 优先显式表达
* 优先可读性

---

# 17. Copilot 生成约束（重要）

生成代码时必须：

* 使用小接口
* 避免大而复杂的抽象
* 避免不必要的层级
* 避免未使用代码
* 避免过度设计
* 优先符合 Go 习惯
* 优先可组合设计

---

# 18. 核心原则

> 编写易于组合、测试和理解的代码。
