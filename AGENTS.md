# AGENTS.md

## Purpose
このリポジトリで作業するエージェント向けの最優先ガイドです。  
迷った場合は「安全」「小さな差分」「再現可能な検証」を優先してください。

## Priority
1. ユーザー / 開発者からの直接指示
2. この `AGENTS.md`
3. `docs/*.md` の詳細ドキュメント

## Project Stack
- Frontend: Vite + TypeScript
- Backend: Hono + TypeScript
- Package manager: pnpm
- Runtime / tools management: mise
- Lint / format: Biome
- Unit test: Vitest（FE / BE）
- E2E test: Playwright CLI
- IaC: Terraform / Terragrunt（採用時）

## Core Rules (Always)
- 変更は目的に対して最小差分で実施する（無関係な変更を避ける）。
- 既存構成・既存命名を優先し、勝手な大規模リファクタはしない。
- 破壊的変更・仕様変更は、理由と影響範囲を明示する。
- 実装後は lint / format / test を実行し、結果を記録する。
- コマンドや設定を追加したら、ドキュメントを必ず更新する。

## Required Checks
詳細: `docs/dev-commands.md`  
最低限、以下を通すこと:
- `pnpm biome check .`
- `pnpm biome format --write .`
- `pnpm test`

## Coding Standards
詳細: `docs/code-style.md`

## Architecture & Delivery
詳細: `docs/architecture.md`

## Database
詳細: `docs/database.md`

## Infrastructure
詳細: `docs/infra.md`

## PR Criteria
詳細: `docs/pr-checklist.md`

## Testing Strategy
詳細: `docs/testing-strategy.md`

## Error/Logging & Release
詳細: `docs/ops-observability.md`, `docs/release-migration.md`

## Scope & Nesting Policy
- 参照先は `docs/` 直下のガイドに限定する。
- 原則として参照の深さは1段まで（孫参照を作らない）。
- 重要な禁止事項・必須チェックはこのファイルにも明記する。
- ルール衝突時はこの `AGENTS.md` を優先する。

## AGENTS.md vs Skills Policy
- **AGENTS.md に書くもの**: 常に守るべき判断基準、必須チェック、禁止事項、優先順位。
- **Skills に書くもの**: 手順が長い作業、繰り返し実行するワークフロー、外部ツール連携手順。
- 目安として、1回の変更で毎回参照が必要な内容は AGENTS.md、特定タスク時のみ必要な詳細は Skills に置く。
