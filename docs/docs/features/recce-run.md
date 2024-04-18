---
title: Run
icon: material/chevron-right-box
---


Recce provides a Web UI for interactively exploring and comparing two environments at a deep level. However, sometimes using a command line interface is more suitable.

1. In the CI workflow, comparing the results of PRs with production or staging environments and obtaining basic information.
1. During development, executing checks that are already source controlled.

## Execute the recce

```shell
recce run
```

or 

```shell
recce run -o recce_pr123.json
```

The command would

1. Collect the dbt artifacts for both base and current environments
2. **Run the default checks:** The default checks is to run the row count diff for the modified table models
3. **Run the preset checks:**
4. Output the result. The default path is `recce_state.json`
