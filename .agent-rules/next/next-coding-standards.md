# Next.js コーディング規約（App Router 前提）

## 基本姿勢
- **Server Components を既定**とし、`use client` は必要最小限。
- データ取得は `fetch` とキャッシュ/再検証API（`revalidate` / `revalidateTag` / `unstable_cache`）を理解して**意図を明文化**。
- APIは **Route Handlers** を既定とし、`pages/api` との二重運用を避ける。

## 型とツール
- TypeScript **strict**、`tsc --noEmit` をCIに。`eslint-config-next` を採用。

## データ取得とキャッシュ
- GETは既定でキャッシュ対象。**ユーザー依存データ**は `cache: 'no-store'`。
- **ISR** は `revalidate`、**オンデマンド**は `revalidatePath` / `revalidateTag` を利用。
- タグ付けしたfetchは `revalidateTag` により**横断的に無効化**できる。

## ルーティング
- App Router の **Route Handlers**（`app/**/route.ts`）を使用。HTTPメソッドごとにエクスポート。
- エッジ/Node の実行環境は要件に合わせて選択し、依存の対応可否を明記。

## メタデータ/SEO
- `generateMetadata` で動的メタデータを生成。`next/image`・OG画像生成を活用。

## アクセシビリティ
- `eslint-plugin-jsx-a11y`、自動テストでのa11yチェック（Playwright/axe）。

## エラーハンドリング
- `error.tsx` / `not-found.tsx` を活用し、ユーザーに可視な失敗を丁寧に扱う。
