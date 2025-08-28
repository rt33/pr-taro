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

## ⚡ パフォーマンス
- **計測に基づく**改善以外は採用しない（pprof/ベンチの根拠必須）。
- ホットパスのアロケ削減、I/Oのストリーム化。

## 🏗 設計
- 公開API最小化、インターフェースは最小メソッド集合。
- テスト容易性（依存注入、table-driven、Fuzz/PBT）。

## 🧪 テスト
- 単体→統合→E2Eの順。ベンチは `B.Loop` を推奨、`benchstat` で差分提示。

## 🗃 DB
- `QueryContext/ExecContext` 徹底、Txは `defer rollback` パターンを基本に。
