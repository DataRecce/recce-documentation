---
title: Share Recce Session
icon: material/account-eye
---

## Share your validation results with anyone, no setup needed

If you've already used Recce to validate your PR and prepared checks, but stakeholders might not have the environment to run Recce.

Recce Cloud allows you to share your Recce validation results with full context, using a simple link. Stakeholders can open a read-only Recce view directly in their browser. No installation, no configuration, just instant access.

!!! Note

    Please note that anyone with the link can visit your shared Recce after sign in Recce Cloud. If you need to restrict access, [please contact us](https://cal.com/team/recce/chat).

## How to use

When you're ready to share your lineage exploration, query results, or validation checklist, simply click **Share** in Recce.

The first time you do this, you'll need to connect your local Recce to Recce Cloud. This requires signing in and setting up an API token. Once connected, Recce Cloud will host your state file securely, allowing you to share a link that others can open in their browsers.

1. Sign in to Recce Cloud and get your API token
    
    Please copy your API token from the [personal settings page](https://cloud.datarecce.io/settings#tokens) in Recce Cloud.
    ![Recce API Token](../../assets/images/recce-cloud/setting-page-api-token-fs8.png){: .shadow}

1. Add the token to `.recce/profile.yml`

    For convenience, you can enter your API token in `.recce/profile.yml`, located by default in your home directory. 
    ```yaml
    user_id: <your_user_id>
    api_token: <your_api_token>
    ```
    Or, for one-time use, you can use `--api-token` flag with commend. e.g.,
    ```bash
    recce server --api-token <your_api_token>
    ```

1. Start Recce server

    After launching, you'll see the __Share__ button, then you can click it to get the link on top.
    ![Recce Share From Server](../../assets/images/recce-cloud/recce-share-from-server-fs8.png){: .shadow}

    You can also use the `recce share` command. If you already have a prepared Recce state file, you can obtain a share link directly through the command line.
    ```bash
    recce share <your_state_file>
    recce share --api-token <your_api_token> <your_state_file>  # for one-time use
    ```
    ![Recce Share From CLI](../../assets/images/recce-cloud/recce-share-from-cli.png){: .shadow}
