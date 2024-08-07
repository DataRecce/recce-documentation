---
title: Run
icon: material/chevron-right-box
---

Recce provides a Web UI for interactively exploring and comparing two environments in depth. However, in the CI workflow, using the command line interface is more appropriate.


To execute Recce from the command line:

```shell
recce run
```

or to specify an output file:

```shell
recce run -o recce_pr123.json
```

This command will:

1. Collect the dbt artifacts for both the base and current environments.
1. **Run the [preset checks](./preset-checks.md):** Preset checks are checks that have been configured specifically for this dbt project.
1. Output the result. The default output path is `recce_state.json`.
