---
title: Configuration
icon: octicons/gear-16
---

The config file for Recce is located in recce.yml. Currently, only preset checks are configurable.

### Preset Checks

[Preset checks](../features/preset-checks.md) can be generated when executing `recce server` or `recce run`.

```yaml
# recce.yml
checks:
  - name: Query diff of customers
    description: |
        This is the demo preset check.

        Please run the query and paste the screenshot to the PR comment.
    type: query_diff
    params:
        sql_template: select * from {{ ref("customers") }}
    view_options:
        primary_keys:
        - customer_id
```


| Field  | Description | Type |
| --- | --- | ---|
| `name`  | the title of the check  | string |
| `description`  | the description of the check  | string |
| `type` | the type of the check | string |
| `params` | the parameters for running the check | object |
| `view_options` | the options for presenting the run result | object |


!!!Note

    Regarding the supported types and settings for each type, the documentation does not currently provide this information. Please obtain the settings by acquiring the preset checks template through the [Web UI](../features/preset-checks.md#configure-the-preset-check).