# Testing Strategy

## TL;DR
- テストピラミッドは Unit を厚く、E2E は細く保つ。
- 変更の種類に応じて必要テストを選び、PRで実行結果を明示する。
- 新規関数には境界テストを必須とする。

## Recommended Test Mix
- Unit (Vitest): 70-80%
- Integration (Vitest + API/DB): 15-25%
- E2E (Playwright): 5-10%

## PR Gate by Change Type
- UI表示変更のみ: Unit + 必要最小限のE2E
- API/ドメイン変更: Unit + Integration（必要ならE2E）
- 認証/権限/決済系: Unit + Integration + E2E を必須

## Unit Test Rules
- 正常系・異常系・境界値（min/max/empty/null）を必ず含める。
- 1テスト1責務、失敗理由が読んで分かる名前を付ける。

## E2E Scope
- 主要ユーザーフローのみを対象にする（ログイン、作成、更新、権限）。
- すべてをE2Eで担保しない。回帰はUnit/Integrationに寄せる。
