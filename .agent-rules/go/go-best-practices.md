# Go ベストプラクティス集

## エラー
- `%w` でラップし、呼び先で `errors.Is/As` 判定。

## 並行・キャンセル
- `errgroup.WithContext` で子ゴルーチンを束ね、早期終了とキャンセルを統合。

## HTTP/DB
- サーバは Timeouts 4点セット、クライアントは `Transport` 明示。
- プリペアド＋Tx・再試行方針を**要件とセット**で文書化。

## テスト例
```go
// Table-driven
func TestSlug(t *testing.T) {
  cases := []struct{name, in, want string}{
    {"ascii", "Hello, World!", "hello-world"},
    {"jp", "日本 語", "日本-語"},
  }
  for _, c := range cases {
    t.Run(c.name, func(t *testing.T) {
      if got := Slug(c.in); got != c.want {
        t.Fatalf("got=%q want=%q", got, c.want)
      }
    })
  }
}

// Go 1.24+ Benchmark
func BenchmarkFoo(b *testing.B) {
  // setup...
  for b.Loop() {
    Foo()
  }
}

// Fuzz
func FuzzParse(f *testing.F) {
  f.Add("seed")
  f.Fuzz(func(t *testing.T, in string) {
    _ = Parse(in)
  })
}
```
