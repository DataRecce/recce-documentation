---
title: Recce CI with GitHub Action
icon: material/github
---

# Recce CI integrate with GitHub Action

Recce provide `recce run` command for CI/CD pipeline. You can integrate Recce with GitHub Action to compare the data models between two environments when a new pull-request is created.

## Prerequisites

Before you start integrating Recce with GitHub Action, you need to have the following prerequisites:

- Set up two environments in your data warehouse. For example, one for production and another for development.

- Provide the credentials profile for both environments in your `profiles.yml` file to let Recce access your data warehouse. You can put the credentials in the `profiles.yml` file. Or you can use the environment variables to provide the credentials.

- Set up the data warehouse credentials in the GitHub repository secrets. You can set up the credentials in the GitHub repository secrets by following the steps mentioned in the [GitHub documentation](https://docs.github.com/en/actions/reference/encrypted-secrets).

## Set up Recce with GitHub Action

We will suggest setting up two GitHub Actions workflows in your GitHub repository. One for the production environment and another for the development environment.

For the production environment, it will be triggered on every merge to the main branch.

And for the development environment, it will be triggered on every push commits to the pull-request branch.

### Base Workflow (Main Branch)

In this workflow, we will set up the GitHub Action to run the dbt commands for the production environment. And then, it will package the dbt artifacts and upload them to the 3rd party storage system outside the GitHub. We will use the AWS S3 bucket to store the dbt artifacts here.

```yaml
name: Recce CI Base Branch

on:
  push:
    branches:
      - main

concurrency:
  group: recce-ci-base
  cancel-in-progress: true

jobs:
  build:
    name: DBT Runner
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
          dbt seed --target ${{ env.DBT_BASE_TARGET }} --target-path target-base
          dbt run --target ${{ env.DBT_BASE_TARGET }} --target-path target-base
          dbt docs generate --target ${{ env.DBT_BASE_TARGET }} --target-path target-base
        env:
          # Set the dbt target name of the base environment
          DBT_BASE_TARGET: prod

      - name: Package DBT artifacts
        run: |
          tar -czvf dbt-artifacts.tar.gz target-base
          mv dbt-artifacts.tar.gz $GITHUB_WORKSPACE/${{ github.sha }}.tar.gz

      - name: Upload to S3
        run: |
          aws s3 cp $GITHUB_WORKSPACE/${{ github.sha }}.tar.gz s3://${{ env.AWS_S3_BUCKET }}/${{ github.sha }}.tar.gz
        env:
          # Set these in your repository secrets
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          # Set these in your repository secrets
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          # Set these in your repository secrets
          AWS_REGION: ${{ secrets.AWS_REGION }}
          # Set these in your repository secrets
          AWS_S3_BUCKET: ${{ secrets.AWS_S3_BUCKET }}
```

### Current Workflow (Pull Request Branch)

In the current workflow, we will set up the GitHub Action to run the dbt commands for the development environment. And then, download the dbt artifacts built in the base environment from the 3rd party storage system. After that, it will compare the data models between the base and current environments using Recce.

```yaml
name: Recce CI Current Branch

on:
  pull_request:
    branches: [main]

jobs:
  check-pull-request:
    name: Check pull request by Recce CI
    runs-on: ubuntu-latest
    permissions:
      pull-requests: write
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3
        with:
          fetch-depth: 0

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: "3.10.x"

      - name: Install dependencies
        run: |
          pip install -r requirements.txt

      - name: Install Recce
        run: |
          pip install recce

      - name: Prepare DBT Base environment
        run: |
          if aws s3 cp s3://$AWS_S3_BUCKET/${{ github.event.pull_request.base.sha }}.tar.gz .; then
            echo "Base environment found in S3"
            tar -xvf ${{ github.event.pull_request.base.sha }}.tar.gz
          else
            echo "Base environment not found in S3. Running dbt to create base environment"
            git checkout ${{ github.event.pull_request.base.sha }}
            dbt deps
            dbt seed --target ${{ env.DBT_BASE_TARGET }} --target-path target-base
            dbt run --target ${{ env.DBT_BASE_TARGET }} --target-path target-base
            dbt docs generate --target ${{ env.DBT_BASE_TARGET }} --target-path target-base
          fi
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_REGION: ${{ secrets.AWS_REGION }}
          AWS_S3_BUCKET: ${{ secrets.AWS_S3_BUCKET }}
          # Set the dbt target name of the base environment
          DBT_BASE_TARGET: prod

      - name: Prepare DBT Current environment
        run: |
          git checkout ${{ github.event.pull_request.head.sha }}
          dbt deps
          dbt seed --target ${{ env.DBT_CURRENT_TARGET }}
          dbt run --target ${{ env.DBT_CURRENT_TARGET }}
          dbt docs generate --target ${{ env.DBT_CURRENT_TARGET }}
        env:
          # Set the dbt target name of the current environment
	        DBT_CURRENT_TARGET: dev

      - name: Run Recce CI
        run: |
          recce run --github-pull-request-url ${{ github.event.pull_request.html_url }}

      - name: Archive Recce State File
        uses: actions/upload-artifact@v4
        id: recce-artifact-uploader
        with:
          name: recce-state-file
          path: recce_state.json

      - name: Comment on pull request
        uses: thollander/actions-comment-pull-request@v2
        with:
          message: |
            Recce `run` successfully completed.
            Please download the [artifact](${{ env.ARTIFACT_URL }}) for the state file.
        env:
          ARTIFACT_URL: ${{ steps.recce-artifact-uploader.outputs.artifact-url }}
```

## Review the Recce State File

Once the Recce CI workflow is completed, you can download the Recce state file from the GitHub pull-request. The Recce state file contains the comparison results of the data models between the base and current environments.

```bash
recce server --review recce_state.json
```

In the Recce server review mode, you can review the comparison results of the data models between the base and current environments. It will contain the row counts of modified data models, and the query results of the Recce Preset Checks.
