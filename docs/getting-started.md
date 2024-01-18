---
title: Getting Started
icon: material/rocket-launch-outline
---

# Getting Started

## Prerequisites

Recce requires that your dbt project has two environments to compare. For example, one for production and another for development. 

Prepare two targets with separate [schemas](https://docs.getdbt.com/docs/core/connect-data-platform/connection-profiles#understanding-target-schemas) in your dbt profile. Your `profiles.yml` might look something like this:

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

## Install `Recce`

Install Recce using `pip`:
```bash
pip install -U recce
```

## Use `Recce` in your dbt project

The following instructions give an overview of the process of using Recce in your dbt project. For a hands-on tutorial, please check the Jaffle Shop Demo.

### Navigate to your dbt project.

```
cd your-dbt-project/
```

### Prepare base artifacts

Recce expects two sets of dbt [artifacts](https://docs.getdbt.com/reference/artifacts/dbt-artifacts) to be present:

- `target-base/` - dbt artifacts for production data
- `target/` - dbt artifacts for your development branch

dbt generates artifacts into the `target/` folder with every invocation:


   | artifacts               | dbt command                |
   | ----------------------- | -------------------------- |
   | manifest.json           | `dbt run`, `dbt build`, .. |
   | catalog.json (optional) | `dbt docs generate`        |


Copy the artifacts for the base environment to `target-base/` folder.

Run the recce server.

```bash
recce server
```

Recce would diff environments between `target/` and `target-base/`


