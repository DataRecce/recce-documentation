---
title: Configuration
icon: octicons/gear-16
---

The config file for Recce is located in recce.yml. Currently, only preset checks are configurable.

## Preset Checks

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


| Field  | Description | Type | Required |
| --- | --- | --- | --- |
| `name`  | the title of the check  | string | Yes |
| `description`  | the description of the check  | string | |
| `type` | the type of the check | string | Yes |
| `params` | the parameters for running the check | object | Yes |
| `view_options` | the options for presenting the run result | object |  |


## Check Params
### Row Count Diff

| Field  | Description | Type | Required |
| --- |--- | --- | --- |
| `node_names` | List of node names.  | `string[]` | *1 |
| `node_ids`  | List of node ides   | `string[]` | *1 |
| `select` | The node selection syntax to select. See [more](https://docs.getdbt.com/reference/node-selection/syntax) | `string` | |
| `exclude` | the node selection syntax to exclude. See [more](https://docs.getdbt.com/reference/node-selection/syntax) | `string` | |
| `packages` | The package filter | `string[]` | |
| `view_mode` | The quick filter to select changed model and 1st degree of upstream. | `all`, `changed_model` | |

**Notes**

\*1: If `node_ids` or `node_names` is specified, it will be used; otherwise, nodes will be selected using the criteria defined by `select`, `exclude`, `packages`, and `view_mode`.

**Examples**

Run row count by node selector
```yaml
checks:
  - name: 'name'
    description: 'description'
    type: row_count_diff
    params:
       select: state:modified,config.materialized:table
       exclude: tag:dev 
```

Run row count by node names
```yaml
checks:
  - name: 'name'
    description: 'description'
    type: row_count_diff
    params:
      node_names: ['customers', 'orders']
```

### Schema Diff

| Field  | Description | Type | Required |
| --- |--- | --- | --- |
| `node_id`  | The node id or list of node ids to check.   | `string[]` | *1 |
| `select` | The node selection syntax to select. See [more](https://docs.getdbt.com/reference/node-selection/syntax) | `string` | |
| `exclude` | the node selection syntax to exclude. See [more](https://docs.getdbt.com/reference/node-selection/syntax) | `string` | |
| `packages` | The package filter | `string[]` | |
| `view_mode` | The quick filter to select changed model and 1st degree of upstream. | `all`, `changed_model` | |

**Notes**

\*1: If `node_id` is specified, it will be used; otherwise, nodes will be selected using the criteria defined by `select`, `exclude`, `packages`, and `view_mode`.

**Examples**

Check schema diff by node selector
```yaml
checks:
  - name: 'name'
    description: 'description'
    type: schema_diff
    params:
       select: state:modified+
       exclude: tag:dev 
```

Check schema diff by node ides
```yaml
checks:
  - name: 'name'
    description: 'description'
    type: row_count_diff
    params:
      node_id: model.jaffle_shop.customers
```

### Lineage Diff
| Field  | Description | Type | Required |
| --- |--- | --- | --- |
| `select` | The node selection syntax to select. See [more](https://docs.getdbt.com/reference/node-selection/syntax) | `string` | |
| `exclude` | the node selection syntax to exclude. See [more](https://docs.getdbt.com/reference/node-selection/syntax) | `string` | |
| `packages` | The package filter | `string[]` | |
| `view_mode` | The quick filter to select changed model and 1st degree of upstream. | `all`, `changed_model` | |

**Examples**

```yaml
checks:
  - name: 'name'
    description: 'description'
    type: lineage_diff
    params:
       select: state:modified+
       exclude: tag:dev
```

### Query
| Field  | Description | Type | Required |
| --- |--- | --- | --- |
| `sql_template` | 	The SQL statement, templated using Jinja, to be executed | `string` | Yes |

**Examples**

```yaml
checks:
  - name: 'name'
    description: 'description'
    type: query
    params:
       sql_template: select * from {{ ref("customers") }}
```


### Query Diff
| Field  | Description | Type | Required |
| --- |--- | --- | --- |
| `sql_template` | 	The SQL statement, templated using Jinja, to be executed | `string` | Yes |
| `base_sql_template` | The SQL statement to execute in the base environment, if specified.  | `string` |  |
| `primary_keys` | 	The primary keys used to identify a record. | `string[]` | *1 |

*1: If primary_keys is specified, the query diff is performed in the warehouse. Otherwise, the query result (up to the first 2000 records) is returned, and the diff is executed on the client side.

**Examples**

```yaml
checks:
  - name: 'name'
    description: 'description'
    type: query_diff
    params:
      sql_template: select * from {{ ref("customers") }}
      primary_keys:
      - customer_id
```

### Value Diff
| Field  | Description | Type | Required |
| --- |--- | --- | --- |
| `model` | The name of the model. | `string` | Yes |
| `primary_key` | 	The primary key(s) used to uniquely identify a record. | `string` or `string[]` |  Yes|
| `columns` | 	The list of columns to include in the value diff. | `string[]` |  |


**Examples**

Value diff summary
```yaml
checks:
  - name: 'name'
    description: 'description'
    type: value_diff
    params:
      model: customers
      primary_key: customer_id
```

Value diff with detailed rows
```yaml
checks:
  - name: 'name'
    description: 'description'
    type: value_diff_detail
    params:
      model: customers
      primary_key: customer_id
```

### Profile Diff
| Field  | Description | Type | Required |
| --- |--- | --- | --- |
| `model` | The name of the model. | `string` | Yes |

**Examples**
```yaml
checks:
  - name: 'name'
    description: 'description'
    type: profile_diff
    params:
      model: customers
```

### Histogram Diff
| Field  | Description | Type | Required |
| --- |--- | --- | --- |
| `model` | The name of the model. | `string` | Yes |
| `column_name` | The name of the column. | `string` | Yes |
| `column_type` | The type of the column. | `string` | Yes |

**Examples**
```yaml
checks:
  - name: 'name'
    description: 'description'
    type: histogram_diff
    params:
      model: customers
      column_name: customer_lifetime_value
      column_type: BIGINT      
```

### Top-K Diff
| Field  | Description | Type | Required |
| --- |--- | --- | --- |
| `model` | The name of the model. | `string` | Yes |
| `column_name` | The name of the column. | `string` | Yes |
| `k` | Specifies the top-k items to include in the result. | `number` |  (Default 50) |

**Examples**
```yaml
checks:
  - name: 'name'
    description: 'description'
    type: top_k_diff
    params:
      model: customers
      column_name: customer_lifetime_value
      k: 50
      
```
