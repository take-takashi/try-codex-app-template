# Development Commands (Node + TypeScript)

## TL;DR
- 依存管理は `pnpm`、ツール管理は `mise` を使う。
- 変更後は Biome lint/format と Vitest を必ず実行する。
- E2E は Playwright CLI を使い、必要時に実行する。

## Prerequisites
- `mise` がインストール済みであること。
- Node.js / pnpm は `mise` 経由で揃えること。

## Setup
```bash
mise install
pnpm install
```

## Lint / Format (Required)
```bash
pnpm biome check .
pnpm biome format --write .
```

## Unit Tests (Required)
```bash
pnpm test
```

## E2E Tests (As Needed / Before Release)
```bash
pnpm playwright test
```

## Recommended Local Verification Flow
```bash
pnpm biome check .
pnpm biome format --write .
pnpm test
pnpm playwright test
```

## Notes
- コマンド名（`lint`, `format`, `test`, `e2e`）は `package.json` に統一定義すること。
- CI でも同一コマンドを呼び出し、ローカルとの差異を作らないこと。
