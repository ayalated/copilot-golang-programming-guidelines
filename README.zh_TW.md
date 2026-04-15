# Go 編碼與設計規範（供 AI / Copilot 使用）

> 所有生成的程式碼必須遵循以下規則。

---

# 1. 核心原則

* 遵循 **Unix 哲學**
* 遵循 **Go 語言慣用寫法（idiomatic Go）**
* 優先：**簡單 > 明確 > 可組合**

---

# 2. Unix 風格設計（必須）

* 一個函式只做一件事
* 函式需小且專注
* 優先使用組合（composition）
* 設計需可組合（composable）

建議：

```go
func Process(r io.Reader, w io.Writer) error
```

避免：

```go
func ProcessFile(path string) ([]byte, error)
```

---

# 3. 介面設計

* 介面應小（1–2 個方法）
* 在使用方定義介面
* 單一方法使用 `-er` 命名

建議：

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

# 4. 錯誤處理

* 必須明確處理錯誤
* 不可忽略錯誤
* 必須附加上下文

```go
if err != nil {
    return fmt.Errorf("create user: %w", err)
}
```

---

# 5. 函式設計

* 短小且單一職責
* 建議 ≤ 50 行
* 建議 ≤ 3 個參數

---

# 6. 結構體設計

* 禁止 God Struct
* 使用組合
* 避免使用 `interface{}`

---

# 7. 資料流

* 優先使用 `io.Reader` / `io.Writer`
* 避免硬編碼路徑
* 避免全域狀態

---

# 8. 並發

* 不要隨意啟動 goroutine
* 必須可控制、可取消
* 使用 `context.Context`

```go
func Run(ctx context.Context)
```

---

# 9. Channel 使用

* 預設無緩衝或 size=1
* 避免複雜 channel 設計

---

# 10. 時間處理

* 使用 `time.Time` / `time.Duration`
* 不可用 `int` 表示時間

---

# 11. init()

* 盡量避免使用 `init()`

---

# 12. main()

* main 必須最簡
* 不包含業務邏輯

---

# 13. 日誌

* 使用結構化日誌（如 slog）
* 不可使用 `fmt.Println`

---

# 14. JSON / API

* 使用明確結構體
* 避免 `map[string]interface{}`

---

# 15. 資料安全（Uber 實務）

* 不暴露內部可變資料
* 必要時複製資料

```go
return append([]T(nil), data...)
```

---

# 16. 通用規則

* 禁止全域可變狀態
* 禁止隱式副作用
* 優先明確寫法
* 優先可讀性

---

# 17. Copilot 約束（重要）

生成程式碼時：

* 使用小型介面
* 避免過度抽象
* 避免不必要層級
* 避免未使用程式碼
* 避免過度設計
* 遵循 Go 慣用寫法
* 優先可組合設計

---

# 18. 最終原則

> 撰寫易於組合、測試與理解的程式碼。
