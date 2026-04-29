# PR Checklist

## TL;DR
- PR は「目的」「変更点」「影響範囲」「検証結果」を明確に書く。
- 小さくレビュー可能な単位で提出する。
- Biome / Vitest / 必要に応じて Playwright の結果を添える。

## Required PR Contents
- 目的（なぜこの変更が必要か）
- 主な変更点（箇条書き）
- 影響範囲（Frontend / Backend / API / Build / CI）
- 検証結果（実行コマンドと結果）

## Definition of Done
- [ ] 要件を満たす実装になっている
- [ ] 既存機能への副作用を確認した
- [ ] `pnpm biome check .` を実行し成功
- [ ] `pnpm biome format --write .` を実行
- [ ] `pnpm test` を実行し成功
- [ ] 必要に応じて `pnpm playwright test` を実行し結果を記録
- [ ] 追加/変更した関数に対して境界テストを含む単体テストを追加
- [ ] ドキュメント（必要なら AGENTS / docs）を更新

## Review Guidance
- 破壊的変更は明示されているか
- 命名・責務分離・型安全性は守られているか
- テストが変更内容を十分カバーしているか
