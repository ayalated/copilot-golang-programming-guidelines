# Go 项目设计与代码风格约定

> 本项目遵循 **Unix 风格 + Go Idiomatic 风格**。
> 所有代码（包括 AI 生成代码）必须符合以下约定。

---

# 一、核心设计原则

## 1. Unix 风格（强制）

* 每个函数/模块 **只做一件事**
* 优先使用 **小接口**
* 优先 **组合（composition）而不是继承**
* 面向数据流（`io.Reader` / `io.Writer`）
* 所有组件应 **可组合（composable）**

示例：

```go
func Process(r io.Reader, w io.Writer) error
```

避免：

```go
func ProcessFile(path string)
```

---

## 2. 单一职责（Single Responsibility）

* 一个函数只负责一个明确逻辑
* 一个 struct 不承载多个业务领域

---

## 3. 明确边界（Separation of Concerns）

分层建议：

```
handler → service → repository
```

禁止：

* handler 直接操作数据库
* service 写 HTTP 逻辑

---

# 二、接口设计规范

## 1. 小接口原则（强制）

```go
type Reader interface {
    Read([]byte) (int, error)
}
```

避免：

```go
type MegaInterface interface {
    Read()
    Write()
    Close()
    Process()
}
```

---

## 2. 接口定义在“使用方”

```go
// good
type Store interface {
    Save(User) error
}
```

不要在实现包定义接口。

---

## 3. 接口命名

* 单方法接口：`-er` 后缀

```go
Reader
Writer
Closer
```

---

# 三、错误处理规范

## 1. 显式错误返回（强制）

```go
if err != nil {
    return err
}
```

禁止：

* panic 作为业务流程
* 忽略错误

---

## 2. 错误要有上下文

```go
return fmt.Errorf("create user failed: %w", err)
```

---

## 3. 不要吞掉错误

```go
_ = doSomething() ❌
```

---

# 四、函数设计规范

## 1. 函数应短小

建议：

```text
≤ 50 行
```

---

## 2. 参数不要过多

建议：

```text
≤ 3 个参数
```

否则使用 struct。

---

## 3. 返回值尽量简单

避免：

```go
func Do() (a, b, c, d, e, f int)
```

---

# 五、结构体设计

## 1. 避免 God Struct

```go
type App struct {
    DB
    Redis
    Logger
    Config
}
```

应拆分。

---

## 2. 使用组合而不是继承

```go
type Service struct {
    repo Repository
}
```

---

## 3. 字段必须有明确含义

避免：

```go
Data interface{}
```

---

# 六、命名规范

## 1. 包名

* 小写
* 无下划线
* 简短

```text
user, auth, repo
```

---

## 2. 变量名

* 短但语义清晰

```go
id, err, ctx
```

---

## 3. 避免冗余

```go
user.UserService ❌
```

---

# 七、并发规范

## 1. 不要滥用 goroutine

只在需要时使用。

---

## 2. 必须可控

* 使用 context
* 可取消

---

## 3. 避免 goroutine 泄漏

必须：

```go
select {
case <-ctx.Done():
    return
}
```

---

# 八、Context 使用规范

## 1. 必须作为第一个参数

```go
func Do(ctx context.Context)
```

---

## 2. 不要存入 struct

---

## 3. 不要滥用 Value

---

# 九、日志规范

## 1. 使用结构化日志

```go
slog.Info("create user", "user_id", id)
```

---

## 2. 不要用 fmt.Println

---

# 十、JSON / API 规范

## 1. 明确字段

```go
Name string `json:"name"`
```

---

## 2. 避免 interface{}

---

## 3. 时间统一格式（RFC3339）

---

# 十一、依赖管理

## 1. 禁止随意 replace

仅用于：

* 本地开发
* 临时调试

---

## 2. 必须固定版本

```go
require xxx v1.x.x
```

---

# 十二、测试规范

## 1. 必须可测试（Testable）

* 依赖注入
* 不依赖全局变量

---

## 2. 使用 mock

---

## 3. 测试命名

```go
TestCreateUser
```

---

# 十三、Copilot 使用约束（重点）

Copilot 生成代码必须符合：

* 小接口
* 不生成 God Object
* 不使用全局变量
* 不写过长函数
* 不忽略错误
* 不引入不必要依赖

---

# 🧠 一句话原则

> 写可以被组合的代码，而不是只能被使用一次的代码。
