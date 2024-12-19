---
title: Installation
icon: material/package-variant
---

# Installation

Install `Recce` in your dbt project with pip:

```
pip install recce
```

To take full advantage of all the features of `Recce`, ensure that [dbt_profiler](https://hub.getdbt.com/data-mie/dbt_profiler/latest/) and [audit-helper](https://hub.getdbt.com/dbt-labs/audit_helper/latest/) are installed via the `packages.yml` file in your dbt project .

1. Add these two packages in the packages.yml
2. Do `dbt deps` to install these 2 packages.
   
```yaml 
packages:
  - package: dbt-labs/audit_helper
    version: <version>
  - package: data-mie/dbt_profiler
    version: <version>

```

For full instructions on using `Recce`, check the [Getting Started](get-started.md) guide.