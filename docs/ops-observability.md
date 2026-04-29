# Error Handling & Logging

## TL;DR
- fail-fastで例外化し、握りつぶしや暗黙フォールバックを禁止する。
- 例外はHTTPステータスに明示マッピングし、ログは構造化JSONで出力する。
- すべてのリクエストに相関IDを付与し、追跡可能にする。

## Error Mapping (Backend)
- Validation / Contract error: `400`
- AuthN/AuthZ error: `401` / `403`
- Not found: `404`
- Conflict / business rule violation: `409`
- Unexpected internal error: `500`

## Logging Fields (Minimum)
- `timestamp`
- `level`
- `service` (`frontend` / `backend` / `auth`)
- `env` (`local/dev/stage/prod` etc.)
- `requestId` (相関ID)
- `userId` (取得できる場合)
- `route`
- `errorCode`
- `message`

## Operational Rules
- PII/秘密情報はログに出力しない。
- 例外ログはstacktraceを保持する（本番はアクセス制御下）。
- `warn` は運用判断が必要な状態、`error` は即時対応対象として定義する。
