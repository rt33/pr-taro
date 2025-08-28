# セキュリティ共通チェックリスト

## 依存・機密
- [ ] **最小依存**（まず標準で届くか）。追加時はリスク・運用コストをPRで明記
- [ ] **脆弱性チェック**：Goは `govulncheck ./...`、Nodeは `pnpm audit`
- [ ] **Secrets管理**：環境変数／Vaultなど。**直書き禁止**

## 入出力境界
- [ ] **XSS**：出力エスケープ、`dangerouslySetInnerHTML` 最小化
- [ ] **SSRF**：外部リクエストはallowlist/スキーマ制限/メタデータIP遮断
- [ ] **CSRF/認可**：状態変更系はトークン・SameSite、サーバ側で必ず認可チェック
- [ ] **ログ**：PII/トークンを残さず、構造化ログ＋レベル運用

## Go 特有
- [ ] `http.Server` タイムアウト（Read/Write/Idle/ReadHeader）
- [ ] `context` の期限/キャンセル伝播
- [ ] `database/sql` は**必ずプレースホルダ**／Txの明示／接続プール設定

## Next.js 特有
- [ ] データ取得の**キャッシュ/再検証方針**が明記（`cache` / `revalidate` / `tags`）
- [ ] Route Handlers での**入力検証**・CORS・レート制限
- [ ] 画像/ファイルの**拡張子・MIME**・サイズ制限・署名URL運用
