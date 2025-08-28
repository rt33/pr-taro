# Go コーディング規約（Google Go Style 準拠）

## 命名・構造
- **パッケージ名**は短小・意味的。公開APIは最小化し、**コメントで意図**を記す。
- `internal/` に実装詳細を閉じ込め、境界はインターフェースで薄く切る。

## エラーとコンテキスト
- ラップは `fmt.Errorf("context: %w", err)`、判定は `errors.Is/As`。
- `context.Context` は外部境界から最深部まで伝搬。`TODO()` 常用禁止。

## 並行処理
- `errgroup.WithContext` で**エラー収束＋キャンセル**。共有資源ごとにロック粒度を明確化。
- `-race` を常時有効化し、CIで検知。

## ロギング・観測
- 構造化ログ（JSON）＋相関ID。`pprof/trace` を開発環境で常設。

## HTTP / DB
- `http.Server` タイムアウトを必ず設定。クライアントは `Transport` を明示。
- **SQLは必ずプレースホルダ**。Txは**明示開始/終了**、接続プールは根拠ある値で設定。

## テスト
- Table-driven & サブテスト。外部境界は fakes を先に用意。
- ベンチ：**Go1.24+ は `testing.B.Loop`**、旧環境は `b.N`。
- Fuzz（1.18+）と PBT（rapid 等）で堅牢性を高める。

## 依存
- **標準ライブラリ優先**。追加依存時はセキュリティと運用コストをPRに明記。
- `govulncheck` をCI必須化（到達性も判断材料に）。
