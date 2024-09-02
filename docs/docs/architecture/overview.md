---
title: Overview
icon: fontawesome/regular/building
---

The core concepts in Recce are [Tasks](#tasks), [Runs](#runs), and [Checks](#checks).

### Tasks
A task represents the implementation of specific functionality. For example, tasks such as `QueryDiffTask` and `ValueDiffTask` demonstrate different types of operational processes.

```
def submit_run(task_type, params) -> Run:
    task = find_task(task_type)
    result, error = task(params)

    return Run(
        type=task_type,
        params=params,
        result=result,
        error=error,
    )
```

A task is responsible for:

1. Implementing the logic of the task, primarily involving accepting parameters and ultimately generating a result.
1. Implementing cancellation.
1. Implementing multi-threading.
1. Reporting progress, including progress percentage and messages.

### Runs

A run documents a single execution of a Task, capturing details such as the type of execution, parameters used, and the final outcomes. There are two methods to execute a run:

1. Directly specify the type along with the corresponding parameters.
    ```shell
    curl 'http://localhost:8000/api/runs' \
    -H "Content-Type: application/json" \
    -d @- <<EOF
    {
        "type": "query_diff",
        "params": {
            "sql_template": "select * from {{ ref('customers') }}"
        }
    }
    EOF
    ```
2. Execute via a specific check.
    ```shell
    curl 'http://localhost:8000/api/checks/e2a44206-8568-44e9-8d36-da8856df477d/run' \
    -H 'Content-Type: application/json' \
    --data-raw '{}'
    ```

In the recce instance, each run is recorded, so in theory, it is possible to access the run history. However, the current UI does not offer this feature. You can see the complete run history from the exported recce state.


### Checks
A Check is an item recorded in the user’s Checklist. A Check can be generated through a run or automatically created from preset checks when the Recce is initiated.

A single Check can execute multiple runs; therefore, in theory, it is possible to obtain the run history of a Check, although the current UI does not yet implement this feature.

Additionally, a Check records information in view_options, which is used to determine how the run results are displayed.

