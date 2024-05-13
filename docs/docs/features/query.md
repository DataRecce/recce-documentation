---
title: Query
icon: material/code-block-tags
---

Query page provides an AdHoc query interface to run arbitrary query or diff the query result between two environments. If you're a dbt user, you can use any dbt macros that are installed in your project.

## Execute Query

```
select * from {{ ref("mymodel") }}
```

1. **Run:** performs query in the current environment
2. **Run Diff:** performs the same query in both environments and diffs the results.
3. **Primary key:** select the primary key(s) used to compare the query results.

!!! note

    If the primary key(s) is specified, the query will occur in the warehouse; otherwise, the query will happen across two environments and the comparison will take place on the client side.

![Recce Query Diff](../../assets/images/features/query-diff.png)

!!! tip

    In Mac, you can use `⌘ Enter` to run a query or use `⌘ ⇧ Enter` to run a query diff.

## Query Result

![Recce Query Diff](../../assets/images/features/query-diff.gif){: .shadow}

- **Primary Key:** When comparison occurs on the client side, we can select the primary key by clicking the `key` icon. The primary key columns are used to be identified as the same record for both sides. If no primary key is specified, the records is compared by the row's index.
- **Pinned Column:** The pinned column would show first in the column list.
- **Changed Only:** By selecting the **Changed only**, we can show only the changed rows and columns. The pinned columns are always shown even they are not changed.

## Shortcut to query a model

1. In the lineage page, select a model
2. Click the **Query** button
3. Then the query page is shown and filled with the query for this model

## Add to Checklist

Click the `+` button in the result pane, then you can add the query result to the checklist.

## How Query Diff Works

In the current version, Recce provides two ways to compare the query result between two environments.

_Query diff occurs in the client side:_

Without providing primary key(s) upfront, AdDoc query compare in the client side. That is, Recce fetches the first 2,000 rows and compare in the client side. The advantage is it has more flexibility to query sql for no PK, especially when column structures differ or no clear primary key exists.
However, the limitation is that we cannot find the mismatched rows in a big query result.

_Query diff occurs in the warehouse:_

With primary key(s) given, it can perform a query diff in the warehouse. It only displays changed, added, or removed rows. Therefore, if only one record is different among a million, that specific record will be visible. Hence, it also reduces the amount of data transferred.

Another similar feature is [Value Diff](../lineage/#value-diff). Value diff is based on a chosen model, so you don’t need to write SQL to operate it, though it naturally offers less flexibility. Additionally, value diff can show a summary or actual diff records, whereas query diff only shows the actual diff records.
