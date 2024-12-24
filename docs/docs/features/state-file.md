---
title: Recce State File
icon: material/file
---

## Introduction

The state file represents the serialized state of a Recce instance. It is a JSON-formatted file containing the following information:

- **Checks**: Data from the checks added to the checklist on the Checklist page.
- **Runs**: Each diff execution in Recce corresponds to a run, similar to a query in a data warehouse. Typically, a single run submits a series of queries to the warehouse and retrieves the final results.
- **Environment Artifacts**: Includes `manifest.json` and `catalog.json` files for both the base and current environments.
- **Runtime Information**: Metadata such as Git branch details and pull request (PR) information from the CI runner.

## How to Save the State File

There are multiple ways to save the state file.

1. **Save from the Web UI**: Click the **Save** button at the top of the app. Recce will continuously write updates to the state file, effectively working like an auto-save feature, and persist the state until the Recce instance is closed. The file is saved with the specified filename in the directory where the recce server command is run.

1. **Export from the Web UI**: Click the **Export** button located in the top-right corner to download the current Recce state to any location on your machine.
   ![](../../assets/images/features/state-file-save.png){: .shadow}

1. **Start Recce from a State File**: You can provide a state file as an argument when launching Recce. If the file does not exist, Recce will create a state file and start with an empty state. If the file exists, Recce will load the state and continue working from it.
   ```
   recce server my_recce_state.json
   ```

1. **Use the `run` Command**: For more complex dbt projects with a CI/CD process, where dbt transformations are executed and results are placed in a PR-specific environment, you can integrate the [recce run](./recce-run.md) command into your workflow. This allows reviewers to easily audit results and decide whether a merge can proceed.
   ```
   # create state file at recce_state.json
   recce run
   ```


## How to Use the State File

The state file can be used in several ways:

1. **Continue the state**: Launch Recce with the specified state file.
   ```
   recce server my_recce_state.json
   ```
1. **Review the state**: Running Recce with the `--review` option enables review mode. In this mode, Recce uses the dbt artifacts in the state file instead of those in the `target/` and `target-base/` directories. This option is useful for distinguishing between development and review purposes.
   ```
   recce server --review my_recce_state.json
   ```   
1. **Import checklist from file**: To preserve favorite checks across different branches, you can import a checklist by clicking the **Import** button at the top of the checklist.
1. **Continue the state from `recce run`**: This will execute the checks in the specified state file.
   ```
   recce run --state-file my_recce_state.json
   ```

## Scenario: Development

In the development workflow, the state file acts as a session for developing a feature. It allows you to store checks to verify the diff results against the base environment.

Common development workflow:

1. Run the recce server without a state file.
    ```
    recce server
    ```
1. Add checks to the checklist. 
1. Save the state by clicking the **Save** or **Export** button.
1. Resume your session by launching Recce with the specific state file.
    ```
    recce server recce_issue_1.json
    ```

![State File For Development](../../assets/images/features/state-file-dev.png)

## Scenario: PR Review

During the PR review process, the state file serves as a communication medium between the submitter and the reviewer.

1. Start the Recce server without a state file.
    ```
    recce server
    ```
1. Add checks to the checklist. 
1. Save the state by clicking the **Save** or **Export** button.
1. Share the state file with the reviewer or attach it as a comment in the pull request.
1. The reviewer reviews the results using the state file

    ```
    recce server --review recce_issue_1.json
    ```

![State File For PR Review](../../assets/images/features/state-file-pr.png)

## Recce Cloud

!!! Note

    Currently, Recce Cloud is still under development. If you are interested, please [sign up for a Recce Cloud invite](https://datarecce.io/) or contact us in the [dbt slack #tool-recce channel](https://getdbt.slack.com/archives/C05C28V7CPP)

Although a state file can store the state, it is not very suitable for recording the latest review status of a PR. Especially since a PR may include the submitter, reviewer, and the automated processes in the CI workflow. The purpose of Recce Cloud is to solve the PR review status management issue.

***Prerequisites***

Before uploading the state file to Recce Cloud, you need to define a password to encrypt the state file. The password is used to encrypt the state file before uploading it to Recce Cloud. It will also be used to decrypt the state file when you download it from Recce Cloud. The password is not stored in Recce Cloud, so you need to keep it safe.

### PR Review Workflow

This is a most common workflow: the submitter pushes commits to the GitHub PR, and the reviewer is responsible for reviewing/auditing the PR. This includes checking code changes, ensuring requirements are met, and assessing whether there is any impact on existing models.

**As a submitter**

1. Push changes to the remote
1. Create PR for review
1. Run dbt to prepare the PR review environment
   ```
   dbt run
   ```
1. Launch recce server for this PR branch
    ```
    recce server --cloud --password <password-to-encrypt-state-file>
    ```
1. Add recce checks for review
1. Leave description and screenshots in the PR comments

**As a reviewer**

1. Checkout the PR branch
1. Launch recce server for this PR branch in the review mode
   ```
   recce server --review --cloud --password <password-to-encrypt-state-file>
   ```
1. If all checks are good, mark all checks as **Approved**. Otherwise, leave comment in the PR comment or the recce check description.

### PR Review Workflow with CI

For more mature projects, we introduce CI automation to standardize the process and reduce human-caused variability or errors.
In the workflow, we will do the following two things in the CI:

Execute dbt to create a PR environment.
Execute recce to update dbt artifacts, rerun check runs, and update the PR status to Recce Cloud.

**As a submitter**

1. Push changes to the remote
1. Create PR for review

**In the CI workflow of the PR push event**

1. The github action workflow is triggered by the push event
1. Checkout the PR branch
1. Fetch the dbt artifacts for the base environment
1. Run dbt for the PR environment
   ```
   dbt run
   ```
1. Run recce for the current PR and upload state to the recce cloud.
    ```
    recce run --cloud --password <password-to-encrypt-state-file>
    ```

**As a submitter and reviewer, collaborate the state in the review mode recce server**

1. Checkout the PR branch
1. Launch recce server for this PR branch in the review mode
   ```
   recce server --review --cloud --password <password-to-encrypt-state-file>
   ```
