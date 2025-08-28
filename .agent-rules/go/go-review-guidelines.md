# Go レビューガイドライン（重要度ラベル共通）

> 出力テンプレと重要度定義は共通テンプレを利用（Critical/High/Medium/Low/Info）。

## 🔒 Webアプリセキュリティ
**🔴 Critical**
- SQLインジェクション（文字連結SQL・未バインドパラメータ）
- SSRF攻撃（外部URL無制限フェッチ・Host/IPチェック欠如）
- 認証・認可欠如、JWT/Session管理不備
- Secrets/API Key露出（ログ・レスポンス・エラー）
- HTTPタイムアウト未設定（DoS脆弱性）

**🔴 High**
- CSRF対策不備（SameSite Cookie・CSRF Token）
- XSS脆弱性（入力値サニタイズ不備・Content-Type設定）
- HTTP Header Security不備（CORS・CSP・HSTS）
- `context` 未伝搬でリクエスト処理追跡不可
- 競合状態（`-race` 検出）・未検査エラー破棄
- **Go 1.25**: nilポインタチェック漏れ（panic発生リスク）

## ⚡ Webアプリパフォーマンス
**🟡 Medium**
- HTTPレスポンス時間測定（pprof）、データベースクエリ最適化
- JSON処理最適化、大量データのストリーミング処理
- Connection Pool設定、Keep-Alive活用
- メモリリーク回避（goroutine・HTTP client・DB connection）
- **Go 1.25**: 本番プロファイルでのPGO最適化

## 🏗 設計
- HTTPハンドラー/ミドルウェアの責任分離、依存注入でテスト容易性確保
- **Go 1.23**: 大量データ処理時のIterator pattern活用（`slices.All`等）

## 🧪 テスト
- HTTPハンドラーテスト（`httptest.Server`）、DBはtestcontainers推奨
- table-drivenテストでエッジケース網羅

## 🗃 DB
- `QueryContext/ExecContext` 徹底、Txは `defer rollback` パターンを基本に。

## 🔧 Webアプリ最適化
**🟡 Medium**
- **Go 1.22**: 大きなstruct値のループでパフォーマンス回帰注意
- **Go 1.24**: 大量データ処理時のmap最適化恩恵（30%高速化）
- **Go 1.25**: PGO活用（本番プロファイルでビルド最適化）
- **Go 1.25**: 新JSON実装での互換性確認（`GOEXPERIMENT=jsonv2`）

**🔵 Info**
- コンテナ環境でのGOMAXPROCS自動調整恩恵
- リクエスト処理でのgoroutineリーク回避
