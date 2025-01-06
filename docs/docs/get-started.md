---
title: Getting Started
icon: material/rocket-launch-outline
---

# Getting Started

## Prerequisites
Recce requires that your dbt project has two separate [environments](https://docs.getdbt.com/docs/environments-in-dbt) that refers to different [schemas](https://docs.getdbt.com/docs/core/connect-data-platform/connection-profiles#understanding-target-schemas). For example, one for production with prod schema and another for development with dev schema.

Recce can get your schema from all data warehouses, so you don't need to update the profiles.yml. However, if you use duckdb, you need to add a separate schemas in your dbt profile so you can run locally.Your `profiles.yml` might look something like this:

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

## Install Recce

Install Recce using `pip`:
```shell
pip install -U recce
```

## Use Recce in your dbt project

The following instructions give an overview of the process of using Recce in your dbt project. For a hands-on tutorial, please check the [Jaffle Shop Tutorial](./get-started-jaffle-shop.md).

Navigate to your dbt project.

```shell
cd your-dbt-project/
```

### Prepare dbt artifacts

Recce expects two sets of dbt [artifacts](https://docs.getdbt.com/reference/artifacts/dbt-artifacts) to be present:

- `target-base/` - dbt artifacts for to be used as the base for the comparison e.g. production
- `target/` - dbt artifacts for your development branch

#### Prepare artifacts for the `base` environment
For most data warehouses, you can download the artifacts generated from the codebase of your main branch. You don't need to re-run the whole production in your local. 

However, if you use duckdb, you need to generate the artifacts for the base environment. 
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


## Start the Recce server
Start the Recce server with the follow command:

```shell
recce server
```

Recce use dbt [artifacts](https://docs.getdbt.com/reference/artifacts/dbt-artifacts), which is generated when every invocation. You can find these files in the `target/` folder.

| artifacts     | dbt command                        |
| ------------- | ---------------------------------- |
| manifest.json | `dbt docs generate`, `dbt run`, .. |
| catalog.json  | `dbt docs generate`                |

!!! tip

    The regeneration of the `catalog.json` file is not required after every `dbt run`. it is only required to regenerate this file when models or columns are added or updated.

## First Time Guide for Recce instance
After you start the Recce server, you can see the Recce instance, the Web UI of the active Recce server. 

Here are the 3 steps to use Recce: (see the image below)

1. Click the model you want to check
1. Click “Explore Change”
1. Click “Add to Checklist”
![first time guide of Recce instance](../assets/images/onboarding/material.svg){: .shadow}