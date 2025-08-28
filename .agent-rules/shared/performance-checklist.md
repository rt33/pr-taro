# パフォーマンス共通チェックリスト（計測→改善）

## Go
- [ ] **pprof/trace** でCPU/メモリ/ロックを可視化
- [ ] **アロケーション削減**（hot pathでのコピー回避、Buffer活用）
- [ ] **I/O最適化**（ストリーム/バッファサイズ根拠化）
- [ ] ベンチは **B.Loop（Go1.24+）** を優先

## Next.js
- [ ] **Core Web Vitals**（LCP/INP/CLS）を継続計測（ラボ＋実測）
- [ ] **RSC** を活用しクライアントJSを削減、`use client` は最小限
- [ ] **キャッシュ/再検証設計**：`revalidate`・`revalidateTag`・`unstable_cache`
- [ ] 画像/フォントは `next/image`・`next/font` を正しく使用

## 変更の見せ方
- PRに**計測条件と結果差分**（表やスクショ）を必ず添付。
