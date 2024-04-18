---
title: Development
icon: material/microsoft-visual-studio-code
---

In developing a project with dbt, there are numerous methods available to help you query warehouse data for validation. Recce, in particular, allows for further comparison with production or a specific baseline environment.


## Prepare the environments
In order to enable Recce to compare the base and current environment, you need to prepare artifacts for both environments.

For base environment, put the dbt artifacts in your `target-base/` path. You can have the following options

1. **Download the artifacts from remote storage:** If you use dbt cloud, you can download the latest artifacts in [your production environment](https://docs.getdbt.com/docs/deploy/deploy-environments). For non dbt cloud case, you can upload the latest artifacts to cloud storage (e.g. s3), and write a scripts to download artifacts.
2. **Generate the artifacts for the production environment:**
    ```shell
    dbt docs generate --target prod --target-path target-base/
    ```

For current developing environment, for most of the dbt command, it would generate the `manifest.json`. If you want to update the schema information, you have to run the `dbt docs generate` to generate the `catalog.json`.

Recce also watch the the `target/` and `target-base/` folders. If there is artifact file changed, the recce web ui would reload to the latest version.


## Development Cycle

The common development cycle is

1. Write the code, validate the change, commit your code
2. Push the commits to remote
3. Review the impacts of your changes

Here, I assume your pull request hasn't been marked as "ready for review" yet, and you're still in the process of development, verifying correctness on your own. In this scenario, Recce can assist you in conducting this validation.


### Check the Lineage

DBT provides a method to identify modified models using `dbt ls -s state:modified+,` but this is obviously not usable within dbt docs. While you can determine how many models are affected using this command, you can't visualize these results.

In Recce, you can conduct an initial assessment of your impact scope by [Lineage diff](../features/lineage.md#lineage-diff), which may help you identify potential unintended impacts.

### Validate the Models' Metadata

With lineage diff, you can start from the modified models to confirm your impact. An inexpensive method is to examine the impact scope of the affected models' metadata.

Firstly, you can start by examining [Schema diff](../features/lineage.md#schema-diff) to see if any changes are detected in each model's schema. Sometimes, a change from **Integer** to **Text**, or from **Decimal** to **Numeric**, may have subtle impacts on your downstream models.

Additionally, whether the models with schema changes have only added columns is worth noting, as this might not significantly affect your downstream processes. However, if columns are removed, it's essential to pay special attention to ensure it's the expected outcome.


Next, you can examine the [Row count diff](../features/lineage.md#row-count-diff) for the affected models. Typically, row counts are stored in the warehouse's metadata, meaning you can obtain row count information without much cost. This allows you to quickly determine if row counts are the same or if there are significant changes. Common issues may arise from an erroneous join resulting in unexpected data volumes and erroneous outcomes. Row count diff provides a fast method to identify similar errors.

Observing [the summary of each node](../features/lineage.md#node-summary) can help you quickly review schema and row count changes. By using the lineage diff graph, conducting basic checks on schema and row count, you can already gain a basic level of confidence in the changes made during your development process.

![Node summary](../../assets/images/features/node.png)

### Validate the Column's Summary
Apparently, model metadata alone is insufficient. Sometimes, we need to assess the magnitude of impact that the changes currently in development have on the critical [Marts models](https://docs.getdbt.com/best-practices/how-we-structure/4-marts).

Recce provides 4 powerful diff tools to compare the data level changes.

1. [Value Diff](../features/lineage.md#value-diff): You can use value diff to observe the matched percentage for each column.
2. [Profile Diff](../features/lineage.md#profile-diff): You can use profile diff to compare basic statistical values for each column, such as count, distinct count, min, max, and average.
3. [Histogram Diff](../features/lineage.md#histogram-diff): You can use histogram diff to examine the distribution changes of numeric columns.
4. [Top-K Diff](../features/lineage.md#top-k-diff): You can use top-k diff to analyze distribution changes of categorical columns.

It's important to note that these queries may take longer to execute and require reading larger amounts of data. Please choose the appropriate method based on the data volume of each model.

### Validate by Adhoc Query
If you want to choose the most flexible method, [Query diff](../features/query.md) is the way to go. You can compare individual records, perform complex operations like **where**, **group by**, **order by**. Or even query multiple models with joins.

AdHoc queries also support the use of dbt macros, providing the highest level of flexibility for validation. However, the downside is that you'll need to write the queries yourself.


## Check Driven Development
Test Driven Development (TDD) is a common development pattern where you write tests first, then begin development, validating until the tests pass.

When developing in dbt, of course, you can implement the TDD process through [dbt tests](https://docs.getdbt.com/docs/build/unit-tests). However, writing tests is not the only method. First, tests require very precise validation logic, and second, sometimes we don't want to impact the original data definitions. In such cases, what we want to verify is that the data doesn't change too much, rather than a specific logic.

For example, if we want to make slight adjustments to the definition of "revenue". In the concept of TDD, we would consider what the input data is and what the output should be. But more often, what we want to verify is just ensuring that the changes in revenue for each month are within an expected range.

In recce, we can a simple query for your validation code

```sql
SELECT
    date_trunc('month', order_date) AS month,
    SUM(amount) AS revenue
FROM
    orders
GROUP BY
    month
ORDER BY
    month desc
```

Next, you can add this check to your [checklist](../features/checklist.md). After modifying your code each time, rerun this check until it meets your requirements.

## Store your data

During development, we may inevitably switch branches. If you want to preserve the current state, you can use the Export feature to save it as a file for later loading.


Another technique is to specify the file directly when opening it.
```
recce server recce_issue_123.json
```

When you close the Recce server instance, it saves your current session to this file. The next time you open it using the same file, it will restore to the previous state.

