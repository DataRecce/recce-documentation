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

## Checks Summary

| Total Checks | Data Mismatch Detected |
| ------------ | ---------------------- |
| 5            | 3                      |

### Checks of Data Mismatch Detected

| Name                                       | Type        | Related Models |
| ------------------------------------------ | ----------- | -------------- |
| Model schema of customers                  | Schema Diff | customers      |
| Value diff of customers                    | Value Diff  | customers      |
| Query diff of customers avg lifetime value | Query Diff  | N/A            |
