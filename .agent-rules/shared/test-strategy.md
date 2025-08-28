# テスト戦略（Kent Beck / t-wada）

## 原則
- **RED → GREEN → REFACTOR** を小さく回す。
  - **RED**: まず失敗するテストを書く（実装前にテストから開始）
  - **GREEN**: テストを通す最小限の実装
  - **REFACTOR**: 動作を保ちながらコードを改善
- シンプルデザイン四則（テストが通る／意図が明確／重複排除／要素最小）。
- **単体 > 統合 > E2E** を基本に、価値とリスクで配分を調整。

## Go 実践
- **Table-driven tests** と **サブテスト**（`t.Run`）をデフォルト。
- **データ競合検知**：`go test -race -shuffle=on` をCI必須。
- **ベンチ**：Go1.24+ は `testing.B.Loop`、旧環境は `for i := 0; i < b.N; i++ {}`。
- **Fuzzing**（Go 1.18+）と **Property-based Testing**（rapid等）を境界やパーサに適用。

## Next.js 実践
- **ユニット**：ロジックを関数へ分離、React Testing Libraryで振る舞い中心に。
- **統合**：Route Handlers / Server Actions を fakes（MSW 等）で検証。
- **E2E**：Playwrightで主要フローのみ、過剰なE2Eは避ける。
- **Web Vitals** の計測・回帰検知をパイプラインに組み込み。

## CI 共通
- フレーク検知：Goは `-count=100`、FEはリトライ／安定化。
- カバレッジは「守れる下限」を設定（例: 80%）。例外はPRで明記。
