# Next.js レビューガイドライン（重要度ラベル共通）

## 🔒 セキュリティ
**🔴 Critical**
- `dangerouslySetInnerHTML` の不適切利用（XSS）
- 認可欠如の Route Handlers / Server Actions
- SSRF 可能な外部リクエスト（allowlistなし・生URL）
- Secretsの直書き・環境変数漏えい

**🔴 High**
- キャッシュ方針とデータ性（ユーザー依存/権限依存）の不整合
- `use client` 濫用による機密のクライアント流出・パフォーマンス劣化

## ⚡ パフォーマンス
- 不要なクライアントJS、画像最適化未使用、RSCで解ける箇所のCSR化
- 過剰な `no-store` によるキャッシュミス連発、`revalidateTag` 未活用

## 🏗 設計
- データ境界の責務分離（Route Handlers / Server Components / Client Components）
- エラーハンドリングとUX（`error.tsx` / `not-found.tsx`）

## 🧪 テスト
- RTLでの振る舞い検証、Playwrightでの主要フロー検証
- Web Vitals の回帰監視（ビルド後のラボ＋実測）
