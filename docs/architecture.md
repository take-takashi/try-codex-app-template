# Architecture & Development Approach

## TL;DR
- 要件は「ユーザー価値 → ユースケース → 実装」の順で分解する。
- 駆動開発は **Test-First（TDD寄り）** を基本に、小さなサイクルで進める。
- フロント（Vite）とバックエンド（Hono）は境界を明確にし、共通契約を先に定義する。
- Frontendは mock-first で進め、MSWハンドラをAPI設計の先行版として扱う。

## Design Principles
- まずユースケースを明文化し、成功条件・失敗条件を定義する。
- API仕様（入出力・エラー）を先に合意し、UI/BEを並行実装可能にする。
- 依存方向は「UI -> Application -> Domain -> Infrastructure」を意識する。
- Backend（Hono）はmiddlewareを各APIで再利用し、共通処理を集約する。
- 本質的な実装はビジネスロジックに集中し、Frontend mockの自然言語擬似コードを実装の入力として扱う。
- Frontend/Backend間のリクエスト・レスポンスはZodで検証し、契約不一致を早期検知する。
- Zod schemaは `packages/contracts` に配置し、Frontend/Backendで共通利用する。

## Development Cycle (Recommended)
1. **Plan**: 小さな変更単位の受け入れ条件を定義
2. **Test**: Vitest（必要ならPlaywright）で失敗するテストを先に作る
3. **Implement**: 最小差分で実装
4. **Refine**: 命名・重複・責務を改善
5. **Verify**: Biome / tests / 必要時 E2E を通す

## Frontend Mock-First Policy
- Frontendは最初にMSW mockを作り込み、UI/状態遷移を先に固める。
- MSWでは **ソート・フィルター等の軽量処理のみ** 実装してよい。
- 複雑なビジネスロジックはMSW内に実装しない。
- 複雑ロジックが必要な箇所は、自然言語コメント + 最小限の条件分岐で擬似コード化する。
- 上記コメントは将来のBackend API仕様（設計書）として再利用する。

## Environment Matrix
- `frontend`: `mock` / `real`
- `backend`: `local` / `dev` / `stage` / `prod`
- `auth`: `dummy` / `cloud`

運用ルール:
- 環境差分はフラグ散在ではなく、明示的なENV定義で切り替える。
- `frontend=mock` のときは MSW を必須有効化する。
- `frontend=real` のときは Backend API契約との差分を検証する。

## Folder Structure Baseline
以下は初期テンプレの推奨。実際の規模に応じて調整する。

```txt
.
├─ apps/
│  ├─ web/                 # Vite frontend
│  │  ├─ src/
│  │  │  ├─ app/
│  │  │  ├─ features/
│  │  │  ├─ components/
│  │  │  └─ shared/
│  └─ api/                 # Hono backend
│     ├─ src/
│     │  ├─ routes/
│     │  ├─ application/
│     │  ├─ domain/
│     │  ├─ infrastructure/
│     │  └─ shared/
├─ packages/
│  ├─ contracts/           # API schema / shared types
│  ├─ config/              # tsconfig / biome config preset
│  └─ test-utils/
├─ infra/
│  └─ terragrunt/
└─ docs/
```

## When to Split Packages
- Frontend/Backendの双方で同じ型・バリデーションを使う場合
- CIで独立してテスト・ビルドしたい場合
- 依存境界を明確にして破壊的変更を検出しやすくしたい場合
