# 共通原則（Go / Next.js / レビュー全体）

本ガイドは、**Google Go Style**・**Next.js公式ドキュメント**・Kent Beck / t-wada のテスト思想を土台に、
「計測第一（orisano）・標準優先（mattn）・価値最優先（tokoroten）」の3軸で運用するための共通原則です。

## 1) 判断の優先順位
1. **テストが通ること**（Red→Green→Refactor を小さく回す）
2. **意図の明確化**（命名・構造・コメントは「なぜ」を伝える）
3. **重複排除**（共通化／抽象化は“必要になったら”）
4. **要素最小**（過剰な依存・抽象・層の増殖を避ける）

## 2) 小さく速いフィードバック
- 小粒PR・短いサイクル・高速テストを最優先（t-wada流）。
- 自動レビュー（Claude）と静的解析をPR/Pushで常時回す。

## 3) 依存は最小・標準優先
- **Go**は標準ライブラリと `database/sql` インターフェースを優先。
- **Next.js**はApp Routerの既定（Server Components, Route Handlers, `fetch`のキャッシュ/再検証）を尊重。

## 4) 計測→改善の徹底
- **Go**：`pprof/trace` とベンチ（Go1.24+ は `testing.B.Loop`）で根拠を示す。
- **Next.js**：Core Web Vitals（LCP/INP/CLS）と実測（RUM）で効果を示す。

## 5) セキュリティ前提
- 機密情報の直書き禁止／脆弱依存は早めに解消。
- SSR/Server Actions/Route Handlersでは**認可**・**入力検証**・**外部リクエストのガード（SSRF対策）**を徹底。

## 6) ドキュメント駆動
- 変更の目的・計測方法・結果・影響範囲をPRに明記。
- 「Next.jsの考え方」ドキュメントを **AIレビュー時に常に読ませる**（後述のワークフローで自動化）。
