# Release & Migration Strategy (Drizzle)

## TL;DR
- 開発中はpushごとにmigrationを増やしてよいが、リリース前に整合性を確認する。
- 本番DB変更は Expand/Contract パターンで段階適用する。
- アプリとDBを同時に壊さない順序でデプロイする。

## Development Phase
- スキーマ変更ごとに migration を生成し、PRでレビューする。
- seed / test data / MSW fixture生成ロジックも同時更新する。

## Release Phase (Recommended Order)
1. **Expand**: 後方互換なDB変更（列追加、nullable追加等）を先に適用
2. **Deploy App vNext**: 新旧両方を扱えるアプリをデプロイ
3. **Backfill**: 必要ならデータ移行ジョブを実行
4. **Contract**: 不要列・旧仕様を削除

## Safety Checklist
- [ ] rollback手順がある
- [ ] migration適用時間の見積りがある
- [ ] 大規模更新はバッチ化されている
- [ ] 障害時に読み取り専用モード等の回避策がある
