# Go レビューガイドライン（重要度ラベル共通）

> 出力テンプレと重要度定義は共通テンプレを利用（Critical/High/Medium/Low/Info）。

## 🔒 セキュリティ
**🔴 Critical**
- 文字連結SQL／未バインドパラメータ
- 外部URLを無制限にフェッチ（SSRF）
- 認可欠如・Secrets露出・タイムアウト未設定

**🔴 High**
- `context` 未伝搬／`-race` で検出される競合
- Tx/ロックの粒度不適切、未検査エラー破棄
- 脆弱依存（`govulncheck` 到達）
- **Go 1.25**: nilポインタアクセス前の明示的チェック漏れ（従来動作コードでpanic発生リスク）

## ⚡ パフォーマンス
- **計測に基づく**改善以外は採用しない（pprof/ベンチの根拠必須）。
- ホットパスのアロケ削減、I/Oのストリーム化。
- **Go 1.25**: PGO（Profile-Guided Optimization）の積極活用、本番プロファイル反映
- **Go 1.25**: 新JSON実装（`GOEXPERIMENT=jsonv2`）パフォーマンス検証と互換性テスト

## 🏗 設計
- 公開API最小化、インターフェースは最小メソッド集合。
- テスト容易性（依存注入、table-driven、Fuzz/PBT）。

## 🧪 テスト
- 単体→統合→E2Eの順。ベンチは `B.Loop` を推奨、`benchstat` で差分提示。
- **Go 1.25**: `testing/synctest` 安定版活用、並行処理テストの信頼性向上
- **Go 1.25**: `T.Attr()`/`B.Attr()`でテスト属性設定、並列テスト中の`AllocsPerRun()`禁止
- **Go 1.25**: `T.Output()`メソッドでテスト出力制御、適切なログ管理

## 🗃 DB
- `QueryContext/ExecContext` 徹底、Txは `defer rollback` パターンを基本に。

## 🔧 Runtime・最適化 (Go 1.25)
**🟡 Medium**
- `runtime.AddCleanup()` 並行実行対応、軽量cleanup関数推奨
- `runtime/trace.FlightRecorder` でレアイベント追跡、最小オーバーヘッド
- GOMAXPROCS自動調整（cgroup制限考慮）の動作理解
- DWARF 5デバッグ情報生成、バイナリサイズ・リンク時間最適化

**🔵 Info**
- スライス最適化（スタック割当増加）恩恵確認
- unsafe.Pointerとの組み合わせ時は特に注意
- 実験的GC・encoding/json/v2は明示的opt-in時のみ使用
