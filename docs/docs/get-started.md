---
title: Getting Started
icon: material/rocket-launch-outline
---

# Getting Started

## Prerequisites

`Recce` requires that your dbt project has two environments to compare. For example, one for production and another for development. 

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

Navigate to your dbt project.

```bash
cd your-dbt-project/
```

### Prepare dbt artifacts

`Recce` expects two sets of dbt [artifacts](https://docs.getdbt.com/reference/artifacts/dbt-artifacts) to be present:

- `target-base/` - dbt artifacts for to be used as the base for the comparison e.g. production
- `target/` - dbt artifacts for your development branch

#### Generate artifacts for the `base` environment

Checkout the `main` branch of your project and generate the required artifacts into `target-base`. You can skip `dbt build` if this environment already exists. 

```bash
git checkout main

dbt build --target prod --target-path target-base
dbt docs generate --target prod --target-path target-base/
```

#### Generate artifacts for the `target` environment

```bash
git checkout feature/my-awesome-feature

dbt run
dbt docs generate
```


## Start the `Recce` server
Start the `Recce` server with the follow command:

```bash
recce server
```

If you want to use a previously exported `Recce` state file, specify it when starting the server:

```bash
recce server issue-123.json
```

The `Recce` server will now be accessible at `127.0.0.1:8000`.




