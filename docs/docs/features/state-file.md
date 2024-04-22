---
title: State File
icon: material/file
---

The state file is the serialized state of a recce instance. There are two scenarios where we need to export the state file:

- [PR Review](#pr-review): We can include the state file in the PR Review Comment. If the reviewer wants to further connect to the PR environment, they can use this file to see the final results and perform deeper audits.
- [Development](#development): During development, we often need to save the state and even switch between different branches for development.



## PR Review
In the PR review process, the state file can serve as a medium of communication between the submitter and the reviewer.

![State File For PR Review](../../assets/images/features/state-file-pr.png)

### Create a State File for Review

- **Export from Web UI**: To export the current Recce state, you can use the **Export** button located in the top right corner of the Web UI to export the state to a file.

- **Output of the Recce Run**: 
A more mature dbt project may have a CI/CD process in place where dbt transformations are run, and the results are placed in a PR-specific environment. In such cases, you can integrate [recce run](./recce-run.md) in your automation workflow, making it convenient for reviewers to audit the results to determine whether the merge can proceed.


### What's in the State File

- **Checks**: These are the data from the checks that have been added to the checklist on the Checks page.
- **Runs**: Each execution in Recce represents a run, analogous to a query in the warehouse. However, typically, a single run may submit series of queries in the warehouse and get the final run result.
- **Environments' Artifacts**: The base and current environments' `manifest.json` and `catalog.json`.
- **Runtime Information**: Such as git branch information, PR information in the CI runner.

### Review the State File

To review a PR, you can download the corresponding state file and open the file with additional `--review` option.

```
recce server --review recce_state.json
```

In this mode, the Recce server won't use the dbt artifacts inside `target` and `target-base`. Instead, it will use the artifacts from the Recce state file.

## Development

![State File For Development](../../assets/images/features/state-file-dev.png)

When running the Recce server, you can specify an additional file argument

```
recce server recce_issue_1.json
```

If this file exists, Recce will use it as the initial state. If it doesn't exist, Recce will create a new one. When the server is terminated, the final state will be written into this file.


