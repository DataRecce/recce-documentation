---
title: Continuous Integration (CI)
icon: octicons/play-16
---

# Recce CI integration with GitHub Action

Recce provides the `recce run` command for CI/CD pipeline. You can integrate Recce with GitHub Actions (or other CI tools) to compare the data models between two environments when a new pull-request is created.

The following guide demonstrates how to configure Recce in GitHub Actions.

## Prerequisites

Before integrating Recce with GitHub Actions, you will need to configure the following items:

- Set up **two environments** in your data warehouse. For example, one for production and another for development.

- Provide the **credentials profile** for both environments in your `profiles.yml` so that Recce can access your data warehouse. You can put the credentials in a `profiles.yml` file, or use environment variables.

- Set up the **data warehouse credentials** in your [GitHub repository secrets](https://docs.github.com/en/actions/reference/encrypted-secrets).

## Set up Recce with GitHub Actions

We suggest setting up two GitHub Actions workflows in your GitHub repository. One for the production environment and another for the development environment.

- **Production environment workflow**: Triggered on every merge to the `main branch`. This ensures that production artifacts are readily available for use when a PR is opened.

- **Development environment workflow**: Triggered on every push to the `pull-request branch`. This workflow will compare production models with the current development environment.

### Production Workflow (Main Branch)

This workflow will perform the following actions:

1. Run dbt on the production environment.
2. Upload the generated artifacts to S3 for later use.

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

### Development Workflow (Pull Request Branch)

This workflow will perform the following actions:

1. Run dbt on the development environment.
2. Download previously generated production artifacts from S3.
3. Use Recce to compare the current environment with the downloaded production artifacts.
4. Post the Recce [state file](../features/state-file.md) to a pull request comment.

````yaml
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

      - name: Prepare Recce Summary
        id: recce-summary
        run: |
          recce summary recce_state.json > recce_summary.md
          cat recce_summary.md >> $GITHUB_STEP_SUMMARY
          echo '${{ env.NEXT_STEP_MESSAGE }}' >> recce_summary.md
        env:
          ARTIFACT_URL: ${{ steps.recce-artifact-uploader.outputs.artifact-url }}
          NEXT_STEP_MESSAGE: |
            ## Next Steps
            If you want to check more detail inforamtion about the recce result, please download the [artifact](${{ steps.recce-artifact-uploader.outputs.artifact-url }}) file and open it by [Recce](https://pypi.org/project/recce/) CLI.

            ### How to check the recce result
            ```bash
            # Unzip the downloaded artifact file
            tar -xf recce-state-file.zip

            # Launch the recce server based on the state file
            recce server --review recce_state.json

            # Open the recce server http://localhost:8000 by your browser

      - name: Comment on pull request
        uses: thollander/actions-comment-pull-request@v2
        with:
          message: |
            Recce `run` successfully completed.
            Please download the [artifact](${{ env.ARTIFACT_URL }}) for the state file.
        env:
          ARTIFACT_URL: ${{ steps.recce-artifact-uploader.outputs.artifact-url }}
````

## Review the Recce State File

Review the downloaded Recce [state file](../features/state-file.md) with the following command:

```bash
recce server --review recce_state.json
```

In the Recce server `--review` mode, you can review the comparison results of the data models between the base and current environments. It will contain the row counts of modified data models, and the results of any Recce [Preset Checks](../features/preset-checks.md).
