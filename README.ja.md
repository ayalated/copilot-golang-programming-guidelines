# Go コーディング＆設計ルール（AI / Copilot 向け）

> 生成されるすべてのコードは、以下のルールに従うこと。

---

# 1. 基本方針

* **Unix 哲学**に従う
* **Go のイディオム（idiomatic Go）**に従う
* 優先順位：**シンプル > 明示的 > 合成可能（composable）**

---

# 2. Unix スタイル設計（必須）

* 1つの関数は1つの責務のみ
* 関数は小さく、単一目的にする
* 大きな抽象よりも**組み合わせ（composition）**を優先
* コードは**合成可能（composable）**であること

推奨：

```go
func Process(r io.Reader, w io.Writer) error
```

非推奨：

```go
func ProcessFile(path string) ([]byte, error)
```

---

# 3. インターフェース設計

* インターフェースは**小さく（1～2メソッド）**
* インターフェースは**利用側で定義**
* 単一メソッドは `-er` 命名

推奨：

```go
type Store interface {
    Save(User) error
}
```

非推奨：

```go
type Service interface {
    Create()
    Update()
    Delete()
    Send()
}
```

---

# 4. エラーハンドリング

* エラーは必ず明示的に処理する
* エラーを無視しない
* コンテキスト付きでラップする

```go
if err != nil {
    return fmt.Errorf("create user: %w", err)
}
```

---

# 5. 関数設計

* 小さく、単一責務
* 推奨：50行以内
* 引数は3つ以内が望ましい

---

# 6. 構造体設計

* God Struct を禁止
* composition を使用
* `interface{}` の乱用を避ける

非推奨：

```go
type App struct {
    DB
    Redis
    Logger
}
```

---

# 7. データフロー

* `io.Reader` / `io.Writer` を優先
* ハードコードされたパスを避ける
* グローバル状態を避ける

---

# 8. 並行処理

* goroutine を無制限に生成しない
* すべての goroutine は制御可能であること
* 必ず `context.Context` を使用

```go
func Run(ctx context.Context)
```

---

# 9. Channel の使用

* デフォルトは無バッファ or size=1
* 複雑な channel 設計を避ける

---

# 10. 時間の扱い

* `time.Time` / `time.Duration` を使用
* `int` で時間を表現しない

---

# 11. init()

* `init()` の使用は極力避ける

---

# 12. main()

* main 関数は最小限にする
* ビジネスロジックを書かない

---

# 13. ログ

* 構造化ログを使用（例：slog）
* `fmt.Println` を使用しない

---

# 14. JSON / API

* 明確な構造体を使用
* `map[string]interface{}` を避ける

---

# 15. データ安全性（Uber の実践）

* 内部の可変データを直接公開しない
* 必要に応じてコピーする

```go
return append([]T(nil), data...)
```

---

# 16. 一般ルール

* グローバルな可変状態を禁止
* 暗黙的な副作用を避ける
* 明示的なコードを優先
* 可読性を最優先

---

# 17. Copilot 生成ルール（重要）

コード生成時：

* 小さなインターフェースを使う
* 大きすぎる抽象を避ける
* 不要なレイヤーを作らない
* 未使用コードを生成しない
* 過度な設計を避ける
* Go の慣習に従う
* 合成可能な設計を優先

---

# 18. 最終原則

> 合成しやすく、テストしやすく、理解しやすいコードを書くこと。
