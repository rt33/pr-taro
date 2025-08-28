# Next.js プロジェクト運用ガイド

## 依存とスクリプト（例）
- パッケージ: `pnpm`
- Lint: ESLint（`eslint-config-next`）
- テスト: Vitest/Jest + RTL、E2E: Playwright

```jsonc
// package.json（抜粋）
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "eslint .",
    "typecheck": "tsc --noEmit",
    "test:ci": "vitest run --reporter=dot"
  }
}
```

## CI 実行例
1) `pnpm i --frozen-lockfile`
2) `pnpm run typecheck`
3) `pnpm run lint`
4) `pnpm run test:ci`
5) （必要に応じて）`next build`

## 「Next.jsの考え方」をレビューに常時注入
- `.agent-rules/next/next-thinking.md` と `nextjs-basic-principle/*.md` を **必読** として、
  GitHub Actions がマージ時・コメントトリガで**必ず連結してプロンプトへ渡す**。
