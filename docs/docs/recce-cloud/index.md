---
title: Overview
icon: material/hand-wave-outline
---

!!! Note

    Recce Cloud is currently in **private alpha** and scheduled for general availability later this year.  [Sign up](../../cloud.md#signup) to the Recce newsletter to be notified, or email [product@datarecce.io](mailto:product@datarecce.io) to join our design partnership program for early access.

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
1. Launch the Recce instance in the cloud mode. It will use the dbt artifacts in the local `target` and `target-base` and initiate a new review state if necessary.
   ```
   recce server --cloud
   ```

!!! Note

    Here we assume the you have set the `GITHUB_TOKEN` and `RECCE_STATE_PASSWORD` in your environment variables.


### Review in the cloud mode
If the review state is already available for this PR, you can open the Recce instance to review.

1. Checkout the branch for the reviewed PR.
1. Launch the Recce instance to review this PR
    ```
    recce server --review --cloud
    ```

## Usage

All the commands requires the following settings.

Name           | Environment Variables | CLI Options     | Description                                      
---------------|-----------------------|-----------------|-------------------------------------------------- 
Cloud token    | `GITHUB_TOKEN`        | `--cloud-token` | Used for  <br>1. Get the pull request from github<br>2. Used as the **access token** to the recce cloud 
State password | `RECCE_STATE_PASSWORD`| `--password`    | Used to encrypt/decrypt the state in the recce cloud     

Recce Cloud is used for pull request reviews. Before interacting with the cloud state, you should switch to a branch that has an open PR on the remote.

### Recce server

Initiate the review session of the PR. It would use the local dbt artifacts in the `target/` and `target-base/` directories to sync the state with the cloud.

```shell
git checkout <pr-branch>
recce sever --cloud
```

### Recce server (Review mode)

Review a PR with the remote state.

```shell
git checkout <pr-branch>
recce sever --cloud --review
```

### Recce run

Run or rerun the PR's checks and sync the state with cloud.

```shell
git checkout <pr-branch>
recce run --cloud
```

### Recce summary

Generate the summary markdown

```shell
git checkout <pr-check>
recce summary --cloud > summary.md
```

### Recce cloud

The cloud subcommand in recce provides functionality for managing state files in cloud storage.

#### purge

You can purge the state from your current PR. It is useful when

1. You forgot the password
1. You would like to reset the state of this PR.

```shell
git checkout <pr-branch>
recce cloud purge
```

#### upload

If you already have the state file for the PR, you can upload it to the cloud.

```shell
git checkout <pr-branch>
recce cloud upload <recce-state-file>
```

#### download

You can download the recce state file of the PR from cloud as well.

```shell
git checkout <pr-branch>
recce cloud download
```

## GitHub Pull Request Status Check

Recce Cloud integrate with the [GitHub Pull Request Status Check](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/collaborating-on-repositories-with-code-quality-features/about-status-checks). If there is recce review state synced to a PR, the PR would has a recce cloud check status. Once all checks in recce are approved, the check status would change to passed and ready to be merged.

![alt text](../../assets/images/recce-cloud/pr-checks-all-approved.png){: .shadow}