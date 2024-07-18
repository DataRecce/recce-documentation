---
title: Setup CI in GitHub Actions
icon: octicons/workflow-16
---
In this step, we will set up the automated preparation of the PR environment and execute a recce run and place the results in the recce cloud.

![alt text](../../assets/images/recce-cloud/setup-architecture.png){: .shadow}


## Prerequisites

### DBT Profile

To ensure that each PR has its own isolated environment, it is recommended to put profile.yml under source control in the repository and use environment variables to change the schema name. In the workflow, we can generate the corresponding schema name based on the PR number.

```
myprofile:
  outputs:
    pr:
      type: snowflake
      ...
      schema: "{{ env_var('DBT_SCHEMA') | as_text }}"      
```

### GitHub Token

Currently, we use the GitHub token as the cloud token. Put your github token to the [GitHub Actions Secrets](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions)

In the below example, we use the secret named `RECCE_CLOUD_TOKEN`


!!!Note

    You cannot use the [automatic generated token](https://docs.github.com/en/actions/security-guides/automatic-token-authentication) here, because we need the personal access token (PAT) to verify if the user has PUSH permission of the repository.

### Recce State Password

The Recce State Password is used to encrypt/decrypt the state file before uploading/downloading it to/from Recce Cloud. The password is not stored in Recce Cloud, so you need to keep it safe. You can set the password in the workflow file or in the GitHub Actions Secrets. 

In the below example, we set the password in the GitHub Actions Secrets named `RECCE_STATE_PASSWORD`.

## Workflow Template

````yaml
name: Recce CI PR Branch

on:
  pull_request:
    branches: [main]

jobs:
  check-pull-request:
    name: Check pull request by Recce CI
    runs-on: ubuntu-latest
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
          pip install recce
      - name: Download artifacts for the base environment
        run: |
          gh repo set-default ${{ github.repository }}
          base_branch=${{ github.base_ref }}
          run_id=$(gh run list --workflow ${WORKFLOW_BASE} --branch ${base_branch} --status success --limit 1 --json databaseId --jq '.[0].databaseId')
          echo "Download artifacts from run $run_id"
          gh run download ${run_id} -n target -D target-base
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          WORKFLOW_BASE: ".github/workflows/dbt_base.yml"
      - name: Set schema name for the PR environment
        run: echo "DBT_SCHEMA=PR_${{ github.event.pull_request.number }}" >> $GITHUB_ENV          
      - name: Prepare the PR environment
        run: |
          dbt deps
          dbt seed --target ${{ env.DBT_CURRENT_TARGET}}
          dbt run --target ${{ env.DBT_CURRENT_TARGET}}
          dbt docs generate --target ${{ env.DBT_CURRENT_TARGET}}
        env:
          DBT_CURRENT_TARGET: "pr"

      - name: Run Recce
        run: |
          recce run --cloud --cloud-token ${{ secrets.RECCE_CLOUD_TOKEN }}
        env:
          RECCE_STATE_PASSWORD: ${{ secrets.RECCE_STATE_PASSWORD }}

      - name: Upload DBT Artifacts
        uses: actions/upload-artifact@v4
        with:
          name: target
          path: target/

      - name: Prepare Recce Summary
        id: recce-summary
        run: |
          recce summary --cloud --cloud-token ${{ secrets.RECCE_CLOUD_TOKEN }} > recce_summary.md
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
          RECCE_STATE_PASSWORD: ${{ secrets.RECCE_STATE_PASSWORD }}
          ARTIFACT_URL: ${{ steps.recce-artifact-uploader.outputs.artifact-url }}
          NEXT_STEP_MESSAGE: |
            ## Next Steps          
            If you want to check more detail information about the recce result, please follow this instruction

            ```bash
            # Checkout to the PR branch
            git checkout ${{ github.event.pull_request.head.ref }}

            # Launch the recce server based on the state file
            recce server --review --cloud --password <recce-state-password>

            # Open the recce server http://localhost:8000 by your browser
            ```
      - name: Comment on pull request
        uses: thollander/actions-comment-pull-request@v2
        with:
          filePath: recce_summary.md
          comment_tag: recce
````

## Verify the CI setup

1. Checkout the branch for a PR.
    ```
    gh pr checkout <pr number>
    ```

1. Launch the recce server to review this PR
    ```
    recce server --review --cloud --cloud-token <GITHUB_TOKEN> --password ${{ secrets.RECCE_STATE_PASSWORD }}
    ```