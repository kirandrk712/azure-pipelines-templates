# azure-pipelines-templates

Reusable **Azure Pipelines** YAML for a multi-environment release flow (QA -> UAT -> PRD) that deploys with **SaltStack**.

## Layout

```
azure-pipelines.yml          # stages: Build, QA, UAT, PRD
templates/
  deploy-job.yml             # reusable deployment job (environment-aware)
  salt-apply.yml             # dry-run -> apply -> smoke test
```

## Ideas demonstrated

- **Template reuse** - one deployment job, parameterised per environment.
- **Environment gates** - `deployment` jobs bind to Azure DevOps Environments, so approvals and checks on UAT/PRD need no pipeline changes.
- **Safer rollouts** - Salt `test=True` dry run before applying, batch size (`-b 25%`) for production, and a smoke test after.
- **Manual production switch** - `deployToProd` runtime parameter.
- **Lint in CI** - `yamllint` and `shellcheck` run in the Build stage.

## Setup

1. Create Environments `qa`, `uat`, `prd` and add approvers to `uat`/`prd`.
2. Create SSH service connections: `salt-master-qa`, `salt-master-uat`, `salt-master-prd`.
3. Put your application under `app/` (or adjust the archive step).
4. Pair with [`saltstack-app-deployment`](../saltstack-app-deployment) for the `app` state used here.

> All names (targets, endpoints, paths) are generic placeholders.
