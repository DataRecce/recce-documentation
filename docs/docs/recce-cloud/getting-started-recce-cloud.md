---
title: 5 Minutes Tutorial
icon: material/school
---



## Clone the Jaffle Shop to a Private Repository


**Jaffle Shop** is an example project officially provided by dbt-labs. This document uses [jaffle_shop_duckdb](https://github.com/dbt-labs/jaffle_shop_duckdb) to enable you to start using **recce cloud** from scratch within five minutes.

1. [Create a private repository](https://github.com/new) in your github account.
1. Clone the “Jaffle Shop” dbt data project
   ```shell
   git clone git@github.com:dbt-labs/jaffle_shop_duckdb.git
   cd jaffle_shop_duckdb
   ```
1. Changet the 
   ```
   git remote set-url origin git@github.com:<owner>/<repo>.git
   ```
1. Push it to my new created repository.   
   ```
   git push
   ```

## Prepare the base environment
1. Prepare virtual env
   ```shell
   python -m venv venv
   source venv/bin/activate
   ```
1. Installation
   ```shell
   pip install -r requirements.txt
   pip install recce
   ```
1. Provide additional environment to compare. Edit `./profiles.yml` to add one more target.
   ```diff
   jaffle_shop:
     target: dev
     outputs:
       dev:
         type: duckdb
         path: 'jaffle_shop.duckdb'
         threads: 24
   +    prod:
   +      type: duckdb
   +      path: 'jaffle_shop.duckdb'
   +      schema: prod
   +      threads: 24
   ```
1. Add dbt packages for recce. Add './packages.yml'
   ```
   packages:
   - package: dbt-labs/audit_helper
     version: 0.12.0
   - package: data-mie/dbt_profiler
     version: 0.8.2     
   ```
   and run
   ```
   dbt deps
   ```
1. Prepare production environment
   ```shell
   dbt seed --target prod
   dbt run --target prod
   dbt docs generate --target prod --target-path ./target-base
   ```
1. Add `target-base/` folder to `.gitignore`
   ```diff
    target/
   +target-base/
    dbt_packages/
    dbt_modules/
    logs/
   ```
1. Remove the existing github workflow.
   ```
   rm -rf .github/
   ```
1. Push to the new remote
   ```
   git add .
   git commit -m 'Add recce changes'
   git push
   ```   

## Signup Recce Cloud / Install Recce Cloud App
1. Go to the [recce cloud](https://staging.cloud.datarecce.io/)
    ![alt text](image.png)
    ![alt text](image-1.png)
2. Click the [Install]()
    ![alt text](image-2.png)
    ![alt text](image-3.png)
    ![alt text](image-4.png)
   

## Prepare the review state for PR


1. Checkout a branch
   ```
   git checkout -b feature/recce-getting-started
   ```

1. Prepare development environment. First, edit an existing model `./models/staging/stg_payments.sql`.
   ```diff
   ...

   renamed as (
            payment_method,

   -        -- `amount` is currently stored in cents, so we convert it to dollars
   -        amount / 100 as amount
   +        amount

            from source
   )
   ```
   run on development environment.
   ```shell
   dbt seed
   dbt run
   dbt docs generate
   ```

1. Commit the change
   ```
   git add models/staging/stg_payments.sql  
   git commit -m 'Update the model'
   git push
   ```

1. Create a pull request for this branch.
1. Prepare a [github token](https://github.com/settings/tokens). You have to provide the `repo` permission.
   ![alt text](image-6.png)
      
1. Run the recce server
   ```shell
   export GITHUB_TOKEN=<github-token>
   ```
1. Run recce server in the cloud mode
   ```
   recce server --cloud --password mypassword
   ```
   Open the link http://0.0.0.0:8000, you can see the lineage diff
   ![](../assets/images/jaffle-shop/jaffle_shop_lineage.png)
1. Switch to the **Query** tab, run this query
   ```sql
   select * from {{ ref("orders") }} order by 1
   ```
   Click the `Run Diff` or press `Cmd + Shift + Enter`
   Click on the 🔑 icon next to the `order_id` column to compare records that are uniquely identified by their `order_id`.
   ![](../assets/images/jaffle-shop/jaffle_shop_query.png)
1. Click the `+` button to add the query result to checklist
   ![](../assets/images/jaffle-shop/jaffle_shop_check.png)

1. Terminate the server

## Review the PR
1. Checkout the branch
   ```
   git checkout feature/recce-getting-started
   ```
1. Make sure you have the github token
   ```
   export GITHUB_TOKEN=<token>
   ```    
1. Run the recce server
   ```   
   recce server --cloud --password mypassword --review
   ```