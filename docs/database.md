# Database Guide

## TL;DR
- DB設計はユースケース中心で行い、スキーマ変更は必ずマイグレーションで管理する。
- ORM/マイグレーションは Drizzle を採用する。
- 命名規則・制約・インデックス戦略を先に合意し、実装で逸脱しない。
- アプリケーション層のDTOとDBモデルを分離して、変更耐性を上げる。
- Seedを第一級で設計し、テスト・初期データ投入・MSW fixture生成を共通化する。

## Schema Design
- Drizzle schema を唯一のソースオブトゥルースとして扱う。
- スキーマ変更は手作業ではなく、マイグレーションファイルで実施する。
- 命名規則（table/column/index）を統一し、レビューで確認する。
- 外部キー・一意制約・インデックスはユースケース観点で設計する。

## Data Access
- Repository層を明示し、Domain/Applicationから永続化詳細を隠蔽する。
- N+1や過剰JOINを避けるため、主要クエリは実行計画を確認する。
- 大規模データ更新はジョブ化し、再実行可能な設計にする。

## Drizzle Operations
- `drizzle-kit` で migration を生成し、実行手順を統一する。
- ローカル・CI・本番で同じ migration を適用する（環境別の手作業SQLは禁止）。
- schema変更時は seed とテストデータ生成ロジックも同時更新する。

## Seed Strategy
- Seedは「初期投入用」と「テスト用」で分離しつつ、共通factoryを使って重複を避ける。
- Seedデータは決定的（deterministic）に生成し、実行ごとの揺らぎを避ける。
- エンティティ間参照（user -> org など）はID命名規則を固定し、fixture再利用性を上げる。

## MSW Fixture Reuse
- MSW fixtureは **DB seedの元データ（factory/seed source）を再利用して生成する** 構成を推奨する。
- 実DB挿入用seedと、MSWレスポンス用JSONの生成元を同一にすることで整合性を維持する。
- 変換層（DB model -> API response）を1箇所に置き、API契約変更時の修正点を最小化する。
