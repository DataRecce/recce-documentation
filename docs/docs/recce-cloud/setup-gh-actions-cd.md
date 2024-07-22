---
title: Setup CD in GitHub Actions
icon: octicons/workflow-16
---

In this step, we will set up the automated preparation of the base environment and upload the dbt artifacts to GitHub workflow artifacts, so that the CI workflow can use them.

![alt text](../../assets/images/recce-cloud/setup-architecture.png){: .shadow}

## Workflow Template
!!! note

    Please place the above file in `.github/workflows/dbt_base.yml`. This workflow path will also be used in the next PR workflow. If you place it in a different location, please remember to make the corresponding changes in the next step.

```yaml
name: Recce CI Base Branch

on:
  workflow_dispatch:
  schedule:
    - cron: '0 0 * * *'  
  push:
    branches:
      - main

concurrency:
  group: recce-ci-base
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: "3.10.x"

      - name: Install dependencies
        run: |
          pip install -r requirements.txt

      - name: Run DBT
        run: |
          dbt deps
          dbt seed --target ${{ env.DBT_BASE_TARGET }}
          dbt run --target ${{ env.DBT_BASE_TARGET }}
          dbt docs generate --target ${{ env.DBT_BASE_TARGET }}
        env:
          DBT_BASE_TARGET: "prod"

      - name: Upload DBT Artifacts
        uses: actions/upload-artifact@v4
        with:
          name: target
          path: target/
```

## Verify the CD setup

1. Go to the repository root
1. List the latest run for the workflow
   ```
   gh run list --branch main --workflow .github/workflows/dbt_base.yml
   ```
   ![alt text](../../assets/images/recce-cloud/setup-run-base.png)

1. Download and check the artifact with name `target`
   ```
   mkdir /tmp/artifact
   gh run download 9380631916 -n target -D /tmp/artifact    
   ```
