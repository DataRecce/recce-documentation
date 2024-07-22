---
title: Overview
icon: material/hand-wave-outline
---

!!! Note

    Recce Cloud is currently in **private alpha** and scheduled for general availability later this year.  [Sign up](../../index.md#signup) to the Recce newsletter to be notified, or email [product@datarecce.io](mailto:product@datarecce.io) to join our design partnership program for early access.

## What is `Recce Cloud`?

Recce Cloud is a service specifically designed for streamlining the DBT PR Review workflow. 

Recce Cloud primarily operates through [Recce](../index.md) and integrates GitHub Pull Requests, consolidating the review status of PRs within the Cloud. Without Recce Cloud, we use the [state file](http://localhost:5678/docs/features/state-file/) to store PR review states. However, this method is not very suitable for collaboration or integration with CI because our review states are not stored in a fixed location. Recce Cloud is designed to solve this problem.


## Prerequisite

1. Recce Cloud requires [Recce](../index.md). Please make sure that you have understood how use Recce in your dbt project.
1. Prepare the github personal access token with the `repo` permission. Please see the [GitHub document](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens). And set it to your environment variable.
   ```
   export GITHUB_TOKEN=<token>
   ```
    Or you can set the `--cloud-token <GITHUB_TOKEN>` command option.
1. Prepare the Recce state password. The Recce state password is used to encrypt/decrypt the state file before uploading/downloading. The password is not stored in Recce Cloud, so you need to keep it safe.
   ```
   export RECCE_STATE_PASSWORD=<password>
   ```
   Or you can set the `--password <password>` or `-p <password>` command option.

## Getting Started
The following instructions give an overview of the process of using Recce in your dbt project. For a hands-on tutorial, please check the [Jaffle Shop Tutorial for Cloud](./getting-started-recce-cloud.md).

### Sign Up the Recce Cloud
1. Go to the [recce cloud](https://cloud.datarecce.io/)
2. Sign in by github account
3. Click **Install** button to install Recce Cloud github app to your personal or organization account.
4. Authorize the repositories to the github app.

### Launch the `recce server` in the cloud mode

1. Create a branch for developing.
   ```
   git checkout -b <my-awesome-feature>
   ```
1. Develop your features and prepare the dbt artifacts for the base (`target-base/`) and current (`target/`) environments.
1. Create a pull request for this branch. Recce Cloud requires an open pull request in your GitHub repository. It also stores the latest Recce state for each pull request.
1. Launch the recce server in the cloud mode. It will use the dbt artifacts in the local `target` and `target-base` and initiate a new review state if necessary.
   ```
   recce server --cloud
   ```

!!! Note

    Here we assume the you have set the `GITHUB_TOKEN` and `RECCE_STATE_PASSWORD` in your environment variables.

### Review in the cloud mode
If the review state is already available for this PR, you can open the recce server to review.

1. Checkout the branch for the reviewed PR.
1. Launch the recce server to review this PR
    ```
    recce server --review --cloud
    ```

### Execute the `recce run` in the cloud mode


In general, recce run is used in CI/CD or automation environments. We can use recce run to generate the review state and perform checks. Since it is an automated environment, we assume that the pull request already exists and that the automated process triggers recce run.

1. Checkout the branch
1. Prepare dbt artifacts for the base (`target-base/`) and current (`target/`) environments.
1. Execute the recce run in the cloud mode. It will use the dbt artifacts in the local `target` and `target-base` and initiate a new review state if necessary.
   ```
   recce run --cloud
   ```    