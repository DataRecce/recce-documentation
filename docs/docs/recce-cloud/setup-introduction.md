---
title: Introduction
template: embed.html
---

Recce Cloud is a service specifically designed for DBT PR Review. To achieve a more automated integration of Recce Cloud, this document briefly introduces how to integrate Recce Cloud with a GitHub repository.

In the current version, we need to use two GitHub services, which are GitHub Actions and GitHub Codespaces.

- **GitHub Actions**: We need to set up Workflows to automatically prepare the Base and PR Environments required by Recce. Additionally, the manifests of the generated environments and the state files for Recce should be placed in the [workflow artifacts](https://docs.github.com/en/actions/using-workflows/storing-workflow-data-as-artifacts).

- **GitHub Codespaces**: We use GitHub Codespaces to prepare a reusable Runtime, execute Recce within it, and port forward to use the Recce server in the cloud.

The following diagram describes the entire architecture.

![alt text](../../assets/images/recce-cloud/setup-architecture.png){: .shadow}

Next, we will guide you through the process of integrating Recce Cloud with your GitHub repository in three steps.

- **Setup GitHub Codespaces**: Upon completing this step, you can create an codespace instance on the GitHub repository page or PR page that allows you to use dbt and recce to connect to your data warehouse.
- **Setup CD in GitHub Actions**: Upon completing this step, you can generate the latest base environment and place the dbt artifacts in the workflow run artifacts.
- **Setup CI in GitHub Actions**: Upon completing this step, you can automatically prepare the PR environment when a PR push event occurs, and place the dbt artifacts and recce state file in the run artifacts. Additionally, use Codespaces to automatically start the Recce server to review the PR.