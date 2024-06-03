---
title: Recce Cloud - Setup CI in GitHub Actions
template: embed.html
---

# Recce Cloud - Setup CI in GitHub Actions

````yaml
name: OSO Recce CI

on:
  workflow_dispatch:
  pull_request:
    branches: [main]

env:
  # dbt env variables used in your dbt profiles.yml
  DBT_PROFILES_DIR: ./
  DBT_GOOGLE_PROJECT: ${{ vars.DBT_GOOGLE_PROJECT }}
  DBT_GOOGLE_DATASET: ${{ vars.DBT_GOOGLE_DATASET }}
  DBT_GOOGLE_KEYFILE: /tmp/google/google-service-account.json
  KEYFILE_CONTENTS: ${{ secrets.KEYFILE_CONTENTS }}

jobs:
  check-pull-request:
    name: Check pull request by Recce CI
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: "3.12.x"

      - name: Install dependencies
        run: |
          pipx install poetry==1.7.1
          poetry install
          poetry run which dbt

      - name: Install Recce
        run: poetry run pip install recce-nightly

      - name: Add packages.yml file
        run: |
          echo '${{ vars.PACKAGES_YAML }}' > packages.yml

      - name: Prep Google keyfile
        run: |
          mkdir -p "$(dirname $DBT_GOOGLE_KEYFILE)" 
          echo "$KEYFILE_CONTENTS" > $DBT_GOOGLE_KEYFILE

      - name: Prepare dbt Base environment
        run: |
          run_id=$(gh run list --workflow "OSO Recce Staging CI" --repo DataRecce/oso --status success --limit 1 --json databaseId --jq '.[0].databaseId')
          gh run download $run_id --repo DataRecce/oso
          mv dbt-artifacts target-base
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}

      - name: Set PR Schema
        run: echo "DBT_GOOGLE_DEV_DATASET=OSO_PR_${{ github.event.pull_request.number }}" >> $GITHUB_ENV

      - name: Prepare dbt Current environment
        run: |
          source $(poetry env info --path)/bin/activate
          dbt deps
          dbt build --target ${{ env.DBT_CURRENT_TARGET}}
          dbt docs generate --target ${{ env.DBT_CURRENT_TARGET}}
        env:
          DBT_CURRENT_TARGET: "dev"

      - name: Run Recce CI
        run: poetry run recce run

      - name: Archive Recce State File
        uses: actions/upload-artifact@v4
        id: recce-artifact-uploader
        with:
          name: recce-state-file
          path: recce_state.json

      - name: Prepare Recce Summary
        id: recce-summary
        run: |
          source $(poetry env info --path)/bin/activate
          recce summary recce_state.json > recce_summary.md
          cat recce_summary.md >> $GITHUB_STEP_SUMMARY
          echo '${{ env.NEXT_STEP_MESSAGE }}' >> recce_summary.md

          # Handle the case when the recce summary is too long to be displayed in the GitHub PR comment
          if [[ `wc -c recce_summary.md | awk '{print $1}'` -ge '65535' ]]; then
            echo '# Recce Summary
          The recce summary is too long to be displayed in the GitHub PR comment.
          Please check the summary detail in the [Job Summary](${{github.server_url}}/${{github.repository}}/actions/runs/${{github.run_id}}) page.
          ${{ env.NEXT_STEP_MESSAGE }}' > recce_summary.md
          fi

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
            ```

      - name: Comment on pull request
        uses: thollander/actions-comment-pull-request@v2
        with:
          filePath: recce_summary.md
````
