# Recce Summary

## Lineage Graph

```mermaid
graph LR
model.jaffle_shop.customers["customers

[What's Changed]
Code, Schema, Value Diff"]
style model.jaffle_shop.customers stroke:#ffa502
model.jaffle_shop.customers---->model.jaffle_shop.customer_segments
model.jaffle_shop.customers---->model.jaffle_shop.customer_order_pattern
model.jaffle_shop.customer_segments["customer_segments"]
model.jaffle_shop.customer_order_pattern["customer_order_pattern"]
```

## Impacted Checks

| Name                                       | Type        | Description                                                     | Type of Changes       |
| ------------------------------------------ | ----------- | --------------------------------------------------------------- | --------------------- |
| Model schema of customers                  | Schema Diff | The schema of customers should not be changed                   | Dictionary Item Added |
| Value diff of customers                    | Value Diff  | The customer_lifetime_value in customers should be 100% matched | Column Changed        |
| Query diff of customers avg lifetime value | Query Diff  | The average of customer_lifetime_value should not be changed    | Values Changed        |
