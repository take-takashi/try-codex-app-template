# Infrastructure Guide (AWS + Terraform/Terragrunt)

## TL;DR
- Terraformはstack内に直接リソースを定義し、過度なmodule分割を避ける。
- stack間連携はSSM Parameter Store、適用順はTerragrunt `dependencies` で制御する。
- `plan`時の参照欠落に備え、`mock_outputs` は利用する。

## Terraform / Terragrunt Policy
- AWS前提で `infra/terragrunt/live/*` にstack単位でリソースを定義する。
- 各リソースの重要出力値は Terraform `output` と SSM Parameter Store の両方に出力する。
- stack内の依存は Terraform 側（`depends_on` / data参照）で表現する。
- stack間は Terragrunt `dependencies` で **apply順のみ** を制御し、疎結合を維持する。
- `plan`時にSSM参照が解決できないケースを考慮し、必要に応じて `mock_outputs` を設定する。
- state backend / locking は Terragrunt 側で一元管理する。

## Environment Config Strategy
- 環境差分は `infra/terragrunt/config` に集約する。
- `dev.hcl`, `stage.hcl`, `prod.hcl` の共通設定は `_common.hcl` に定義する。

## Suggested Layout
```txt
infra/
└─ terragrunt/
   ├─ config/
   │  ├─ _common.hcl
   │  ├─ dev.hcl
   │  ├─ stage.hcl
   │  └─ prod.hcl
   └─ live/
      ├─ frontend/
      ├─ backend/
      ├─ auth/
      └─ database/
```
