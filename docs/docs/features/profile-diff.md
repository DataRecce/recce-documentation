---
title: Profile Diff
---

# Profile Diff

Profile diff uses the `get_profile` from [dbt-profiler](https://hub.getdbt.com/data-mie/dbt_profiler/latest/). To use Profile Diff, ensure that dbt-profiler is installed in your project.

```yaml
packages:
  - package: data-mie/dbt_profiler
    version: <version>
```

Please reference [dbt-profiler](https://hub.getdbt.com/data-mie/dbt_profiler/latest/#dbt-profiler) for the definition of the profiling stats.

<figure markdown>
  ![Recce Profile Diff](../../assets/images/features/profile-diff.png)
  <figcaption>Profile Diff</figcaption>
</figure>

