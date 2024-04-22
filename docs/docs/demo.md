---
title: Online Demo
icon: material/keyboard
---

# Recce Online Demo

Try `Recce` without installing using this [online demo](https://pr1.cloud.datarecce.io). 

[Recce Online Demo](https://pr1.cloud.datarecce.io){ .md-button .md-button--primary }

The demo showcases a [pull request](https://github.com/DataRecce/jaffle_shop_duckdb/pull/1) that fixes the `customer_lifetime_value` calculation in dbt's Jaffle Shop project to only included completed orders.

<figure markdown>
  ![customers.sql](../assets/images/demo/clv-customers-model-fs8.png){: .shadow}
  <figcaption>Jaffle Shop customers.sql</figcaption>
</figure>


## Examples

Some example validation checks you might create include:

### Value Diff

Run a [Value Diff](features/lineage.md#value-diff) to check the percentage match of the `customer_lifetime_value` column between production and the development branch.

<figure markdown>
  ![CLV Value Diff](../assets/images/demo/clv-value-diff-fs8.png)
  <figcaption>Value Diff - Customers Model</figcaption>
</figure>

### Profile Diff

Check the [Profile Diff](features/lineage.md#profile-diff) of the customers table to see how the `customer_lifetime_value` has been impacted.

<figure markdown>
  ![CLV Profile Diff](../assets/images/demo/clv-profile-diff-fs8.png)
  <figcaption>Profile Diff - Customers Model</figcaption>
</figure>


### Query Diff
Run a [Query Diff](features/query.md) to compare the the actual values in prod and dev.

```SQL
select customer_id, customer_lifetime_value from {{ ref("customers") }} where customer_id < 50;
```

<figure markdown>
  ![CLV Query Diff](../assets/images/demo/clv-query-diff-fs8.png)
  <figcaption>Query Diff - Customers Model</figcaption>
</figure>


