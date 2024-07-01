---
title: Node Selection
icon: material/select
---

dbt provides the powerful feature of [node selection](https://docs.getdbt.com/reference/node-selection/syntax) in most of its commands. Recce also integrates node selection, allowing you to effectively choose your nodes.

## Supported Features
Currently, row count diff and schema diff support setting node selection within [preset checks](./preset-checks.md). In the near future, the web UI will also support configuring related node selection settings.


### Row Count Diff Check

**Usage**

```
checks:
- name: <name>
  description: <description>  
  type: row_count_diff
  params:
    select: <selection>
    exclude: <selection>
```

**Example: Check row count diff for all modified table models.**

```
checks:
- name: Row count diff
  description: Check the row count diff for all table models
  type: row_count_diff
  params:
    select: state:modified,config.materialized:table
```

**Example: Check row count diff by tag**

```
checks:
- name: Row count diff
  description: Check the row count diff
  type: row_count_diff
  params:
    select: tag:row_count_diff
    exclude: tag:skip_check
```

### Schema Diff Check


**Usage**
```
- name: <name>
  description: <description>
  type: schema_diff
    select: <selection>
    exclude: <selection>
```

On the schema diff result page, it lists all the selected nodes and highlights the nodes with schema changes.
![schema diff with node selection](../../assets/images/features/schema-diff-node-selection.png){: .shadow}

**Example: Check schema diff for all modified nodes and their downstream nodes**
```
- name: Schema diff
  description: Check the schema diff for modified+
  type: schema_diff
  params:
    select: state:modified+
```

**Example: Check schema diff for all nodes**
```
- name: Schema diff
  description: Check the schema diff for all nodes.
  type: schema_diff
  params:
```

## Limitation
In dbt, node selection only lists the nodes currently present in the project, so it does not include removed nodes.

