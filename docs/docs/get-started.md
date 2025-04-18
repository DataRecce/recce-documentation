---
title: Getting Started
icon: material/rocket-launch-outline
---

# Getting started

This guide walks you through how to use Recce, whether you're exploring for the first time or expanding to Recce Cloud.

Use Recce to validate your dbt changes with clarity and confidence. Recce helps you:

- Explore what changed
- Validate downstream impacts
- Collaborate through shareable checklists

For a hands-on walkthrough, check out the [Jaffle Shop Tutorial](https://www.notion.so/infuseai/get-started-jaffle-shop.md).

## Start Recce with two commands

Navigate to your dbt project and run:
```shell
cd your-dbt-project/  # if you're not already there
pip install -U recce
recce server
```

Recce use dbt [artifacts](https://docs.getdbt.com/reference/artifacts/dbt-artifacts) generated with every invocation. You can find these files in the `target/` folder.

| artifacts     | dbt command                        |
| ------------- | ---------------------------------- |
| manifest.json | `dbt docs generate`, `dbt run`, .. |
| catalog.json  | `dbt docs generate`                |

!!! tip

    The regeneration of the `catalog.json` file is not required after every `dbt run`. it is only required to regenerate this file when models or columns are added or updated.

## Get instant visibility

With just one environment, Recce gives you:

- **Lineage clarity**: Trace changes down to the column level

- **Query insights**: Explore logic and run custom queries

- **Live diffing**: Reload and inspect changes as you iterate

Perfect for early exploration, root cause analysis, and faster debugging before involving others.

## Unlock diff & validation with two environments
Not all data changes are obviously right or wrong. Comparing metrics before and after is key to confident validation.

When you configure two dbt environments (e.g. prod and dev schemas), Recce lets you:

- **Explore** modified models and downstream impact

- **Validate** changes via diffs or custom queries

- Add validation results to **Checklists** for review and alignment


### How to setup two environments
Setup two separate [environments](https://docs.getdbt.com/docs/environments-in-dbt) that refers to different [schemas](https://docs.getdbt.com/docs/core/connect-data-platform/connection-profiles#understanding-target-schemas), e.g., prod for production and dev for development.

Recce supports schema discovery across all major warehouses without needing to modify your `profiles.yml`.

However, if you're using DuckDB, you’ll need to explicitly define both schemas to make it run locally. For example:
```yaml
jaffle_shop:
  target: dev
  outputs:
    dev:
      type: duckdb
      path: jaffle_shop.duckdb
      schema: dev
    prod:
      type: duckdb
      path: jaffle_shop.duckdb
      schema: main
```

### Prepare dbt artifacts

Recce expects two sets of dbt [artifacts](https://docs.getdbt.com/reference/artifacts/dbt-artifacts) to be present:

- `target-base/` - dbt artifacts for to be used as the base for the comparison e.g. production
- `target/` - dbt artifacts for your development branch

#### Generate artifacts for the `base` environment
For most data warehouses, you can download the artifacts generated from the codebase of your main branch. 
You don't need to re-run the whole production in your local. However, if you use duckdb, you need to generate the artifacts for the base environment. 

Checkout the `main` branch of your project and generate the required artifacts into `target-base`. You can skip `dbt build` if this environment already exists. 

```shell
git checkout main

dbt run --target prod
dbt docs generate --target prod --target-path target-base/
```

#### Generate artifacts for the `target` environment

```shell
git checkout feature/my-awesome-feature

dbt run
dbt docs generate
```

## Share to collaborate 
If you’ve followed the steps above and are ready to share your checklist with others, Recce Cloud makes it easy.

Just one link gives full context:

- Lineage

- Diff results

- Checklist validation with comments

Built for teams, Recce Cloud includes secure, cloud-hosted sharing and collaboration features designed for fast reviews and confident sign-off.
  
[Sign up for Recce Cloud](/pricing) to unlock collaboration at scale.