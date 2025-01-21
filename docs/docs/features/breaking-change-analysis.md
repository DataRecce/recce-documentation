---
title: Breaking Change Analysis
icon: octicons/diff-modified-24
---

A breaking change is a modification to a model that impacts downstream nodes, potentially altering or disrupting their outputs. When modifying model content, not all changes are considered breaking. Often, these are minor adjustments like formatting or configuration tweaks that do not alter the original SQL results. Breaking change analysis examines modifications to the SQL semantic tree. If no structural differences are detected or the changes involve simply adding new columns, the modification is classified as non-breaking changes.

## Usage

To enable the feature, toggle the switch to **On** in the **Lineage** page. If a modification is identified as a non-breaking change, the corresponding node will appear with an orange dashed border.


=== "Disabled"
    
    ![Breaking Change Analysis (disabled)](../../assets/images/features/breaking-change-disabled.png){: .shadow}

=== "Enabled"
    
    ![Breaking Change Analysis](../../assets/images/features/breaking-change.png){: .shadow}

By default, all modified models are treated as **breaking changes** unless:

1. The semantics are unchanged.
1. The modification only involves adding a new column.



## Caveat

The current implementation of breaking change analysis is still very conservative. As a result, a modified model may be classified as a breaking change when it is actually non-breaking. Common cases include:

1. Adding a column using a `CASE WHEN` statement.
1. Logical equivalence in operations, such as changing `a + b` to `b + a`.
1. Adding a `LEFT JOIN` to a table and selecting columns from it. This is often used to enrich the current model with additional dimension table data without affecting existing downstream tables.
1. All modified python models or seeds are treated as breaking change.


In some cases, breaking changes may be mistakenly identified as non-breaking changes. For example,

1. adding a new column can result in a table expansion effect, similar to the functionality of DuckDB's [unnest function](https://duckdb.org/docs/sql/query_syntax/unnest.html). The unnest function flattens lists or structs, potentially transforming a single upstream row into multiple downstream rows.

## Credit

This feature leverages [sqlglot](https://github.com/tobymao/sqlglot) to analyze SQL and uses its AST diff capability to compare two SQL semantic trees.