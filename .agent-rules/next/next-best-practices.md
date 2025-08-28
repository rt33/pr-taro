# Next.js ベストプラクティス集

## データ取得
```ts
// キャッシュを活かす（タグで横断的に無効化可能）
export async function getPost(id: string) {
  const res = await fetch(`${process.env.API}/posts/${id}`, {
    next: { tags: ['posts', `post:${id}`], revalidate: 60 },
  })
  if (!res.ok) throw new Error('failed to fetch')
  return res.json()
}
```

## Route Handlers
```ts
// app/api/posts/[id]/route.ts
export async function GET(_: Request, { params }: { params: { id: string } }) {
  const post = await getPost(params.id)
  return Response.json(post)
}
```

## メタデータ
```ts
// app/posts/[slug]/page.tsx
export async function generateMetadata({ params }: { params: { slug: string } }) {
  const article = await getArticle(params.slug)
  return { title: article.title, description: article.summary }
}
```

## アクセシビリティ
- フォームは適切なラベル・エラーメッセージを提供。キーボード操作を担保。
- 画像は `alt` 必須、`next/image` を使用。

## パフォーマンス
- Server Components を活用し、クライアントJSを最小化。
- `next/font`・`preload` を適切に使用し、LCPを最適化。
