# Code Style Guide

## TL;DR
- TypeScript の strict 指向を前提に、曖昧な型を避ける。
- Frontend (Vite) / Backend (Hono) で構成を分け、責務分離を守る。
- Lint/format は Biome を唯一の基準にする。
- 実装は t-wada 思想のTDDを基本とし、関数単位で境界テストを必ず追加する。
- コードはシンプルさ優先、エラーは早期例外化しサイレントなフォールバックを避ける。

## TypeScript
- `any` の使用は原則禁止。必要な場合は理由をコメントで明示する。
- 公開関数・重要ロジックには戻り値型を明示する。
- 複雑な union / generic は型エイリアスで名前を付ける。

## Structure
- Frontend と Backend のコードはディレクトリを分離する。
- 共通処理は `shared` など明示的な場所に置き、循環参照を避ける。
- ユーティリティは責務ごとに分割し、巨大ファイル化を防ぐ。

## Naming
- 変数 / 関数: `camelCase`
- 型 / クラス / コンポーネント: `PascalCase`
- 定数: `UPPER_SNAKE_CASE`（必要な場合のみ）

## Error Handling
- 例外は握りつぶさない。ユーザー向けメッセージと開発者向け情報を分離する。
- 想定外入力・契約違反は早期に例外化し、曖昧なフォールバック挙動を入れない。
- API エラーは HTTP ステータスとレスポンス形式を統一する。

## Testing Discipline (TDD)
- 新しい関数を作成したら、同一PRで必ず単体テストを追加する。
- 単体テストには正常系だけでなく境界値・境界条件を含める。
- 失敗するテスト -> 最小実装 -> リファクタ のサイクルで進める。

## Formatting & Linting
- すべて Biome に従う。
- 手動整形より `pnpm biome format --write .` を優先する。
