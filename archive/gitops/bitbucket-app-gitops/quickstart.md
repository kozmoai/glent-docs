---
sidebar_position: 1
---

# Quickstart

:::note Prerequisites

[Please install our Bitbucket app](../../git-provider/bitbucket-exporter/installation.md).
:::

1. Clone our example repo: [bitbucket-app-setup-example](https://bitbucket.com/kozmoai/bitbucket-app-setup-example).

2. Use the `microservice.json` file as a base for your [Blueprint](../../../define-your-data-model/setup-blueprint/setup-blueprint.md).

:::tip
You can add any property you want into the base `microservice.json` file
:::

1. Once you are satisfied with your Blueprint, go ahead and create it in Glint.

2. Now let's get the data inside Glint:

   If you don't have a `glint.yml` file, please create one in your repository in a format that matches the example shown in this [Bitbucket Repository](https://bitbucket.org/kozmoai/bitbucket-app-setup-example/src/master/glint.yml), and then commit/merge it into the `main` branch.

   If you already have a `glint.yml` file in the `main` branch of the cloned example repository, update it to match the Blueprint that you created (the example itself does not require any changes, so if you have just created the Blueprint, without changing `microservice.json`, it will work out of the box).

3. After the changes have been merged, you will see the data specified in the `glint.yml` file appear in the page matching your new Blueprint in Glint!
