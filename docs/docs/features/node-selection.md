---
title: Node Selection
icon: material/select
---

Recce supports **node selection** in certain checks([see below](#supported-checks)). This enables you to target specific resources with data checks by selecting or excluding nodes. Recce node selection follows the same syntax as [dbt node selection](https://docs.getdbt.com/reference/node-selection/syntax).  

## Supported Checks

The following checks currently support node selection:

- Row Count Diff Check
- Schema Diff Check

### Via YML in preset checks
#### Row Count Diff Check

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

#### Schema Diff Check


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

### Via GUI

Currently, node selection for checks can only by configured as preset checks in your `recce.yml`. Support for configuring node selection via the GUI is coming soon.

## Lineage Diff

We will also support using node selection in the [lineage diff](lineage.md#lineage-diff). Stay tuned.

## Limitation
In dbt, node selection only lists the nodes currently present in the project, so it does not include removed nodes.

