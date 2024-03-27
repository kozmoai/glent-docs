import Tabs from "@theme/Tabs"
import TabItem from "@theme/TabItem"

# Bitbucket

import BitbucketResources from './\_bitbucket_exporter_supported_resources.mdx'

Our integration with Bitbucket allows you to export Bitbucket objects to Glint as entities of existing blueprints. The integration supports real-time event processing so Glint always provides an accurate real-time representation of your Bitbucket resources.

## 💡 Bitbucket integration common use cases

Our Bitbucket integration makes it easy to fill the software catalog with data directly from your Bitbucket workspace, for example:

- Map all of the resources in your Bitbucket workspace, including **repositories**, **pull requests** and other Bitbucket objects;
- Watch for Bitbucket object changes (create/update/delete) in real-time, and automatically apply the changes to your entities in Glint;
- Manage Glint entities using GitOps;
- etc.

## Installation

To install Glint's Bitbucket app, follow the [installation](./installation.md) guide.

## Ingesting Git objects

By using Glint's Bitbucket app, you can automatically ingest Bitbucket resources into Glint based on real-time events.

Glint's Bitbucket app allows you to ingest a variety of objects resources provided by the Bitbucket API, including repositories, pull requests and more. The Bitbucket app allows you to perform extract, transform, load (ETL) on data from the Bitbucket API into the desired software catalog data model.

The Bitbucket app uses a YAML configuration file to describe the ETL process to load data into the developer portal. The approach reflects a golden middle between an overly opinionated Git visualization that might not work for everyone and a too-broad approach that could introduce unneeded complexity into the developer portal.

Here is an example snippet from the `glint-app-config.yml` file which demonstrates the ETL process for getting `pullRequest` data from the Bitbucket workspace and into the software catalog:

```yaml showLineNumbers
resources:
  # Extract
  # highlight-start
  - kind: pull-request
    selector:
      query: "true" # JQ boolean query. If evaluated to false - skip syncing the object.
      # highlight-end
    glint:
      entity:
        mappings:
          # Transform & Load
          # highlight-start
          identifier: ".destination.repository.name + (.id|tostring)" # The Entity identifier will be the repository name + the pull request ID. After the Entity is created, the exporter will send `PATCH` requests to update this pull request within Glint.
          title: ".title"
          blueprint: '"bitbucketPullRequest"'
          properties:
            creator: ".author.display_name"
            assignees: "[.participants[].user.display_name]"
            reviewers: "[.reviewers[].user.display_name]"
            status: ".state"
            createdAt: ".created_on"
            updatedAt: ".updated_on"
            link: ".links.html.href"
            # highlight-end
```

The app makes use of the [JQ JSON processor](https://stedolan.github.io/jq/manual/) to select, modify, concatenate, transform and perform other operations on existing fields and values from Bitbucket's API events.

### `glint-app-config.yml` file

The `glint-app-config.yml` file is how you specify the exact resources you want to query from your Bitbucket workspace, and also how you specify which entities and which properties you want to fill with data from Bitbucket.

Here is an example `glint-app-config.yml` block:

```yaml showLineNumbers
resources:
  - kind: repository
    selector:
      query: "true" # JQ boolean query. If evaluated to false - skip syncing the object.
    glint:
      entity:
        mappings:
          identifier: ".name" # The Entity identifier will be the service (repository) name.
          title: ".name"
          blueprint: '"service"'
          properties:
            project: ".project.name"
            url: ".links.html.href"
            defaultBranch: ".main_branch"
```

### `glint-app-config.yml` structure

- The root key of the `glint-app-config.yml` file is the `resources` key:

  ```yaml showLineNumbers
  # highlight-next-line
  resources:
    - kind: repository
      selector:
      ...
  ```

- The `kind` key is a specifier for an object from the Bitbucket API:

  ```yaml showLineNumbers
    resources:
      # highlight-next-line
      - kind: repository
        selector:
        ...
  ```

  <BitbucketResources/>

#### Filtering unwanted objects

The `selector` and the `query` keys let you filter exactly which objects from the specified `kind` will be ingested to the software catalog

```yaml showLineNumbers
resources:
  - kind: repository
    # highlight-start
    selector:
      query: "true" # JQ boolean query. If evaluated to false - skip syncing the object.
    # highlight-end
    glint:
```

For example, to ingest only repositories that have a name starting with `"service"`, use the `query` key like this:

```yaml showLineNumbers
query: .name | startswith("service")
```

<br/>

The `glint`, `entity` and the `mappings` keys open the section used to map the Bitbucket API object fields to Glint entities. The `mappings` key can either be an object or an array of objects that matches the structure of an [entity](../../../sync-data-to-catalog/sync-data-to-catalog.md#entity-json-structure)

```yaml showLineNumbers
resources:
  - kind: repository
    selector:
      query: "true"
    # highlight-start
    glint:
      entity:
        mappings: # Mappings between one Bitbucket API object to a Glint entity. Each value is a JQ query.
          identifier: ".name"
          title: ".name"
          blueprint: '"service"'
          properties:
            project: ".project.name"
            url: ".links.html.href"
            defaultBranch: ".main_branch"
    # highlight-end
```

:::tip
Pay attention to the value of the `blueprint` key, if you want to use a hardcoded string, you need to encapsulate it in 2 sets of quotes, for example use a pair of single-quotes (`'`) and then another pair of double-quotes (`"`)
:::

### Setup

To ingest Bitbucket objects using the [`glint-app-config.yml`](#glint-app-configyml-file) file, you can use one of the following methods:

<Tabs queryString="method">

<TabItem label="Using Glint" value="glint">

To manage your Bitbucket integration configuration using Glint:

1. Go to the DevPortal Builder page;
2. Select a blueprint you want to ingest using Bitbucket;
3. Choose the **Ingest Data** option from the menu;
4. Select Bitbucket under the Git providers category;
5. Add the contents of your `glint-app-config.yml` file to the editor;
6. Click save configuration.

Using this method applies the configuration to all repositories in your Bitbucket workspace.

When configuring the integration **using Glint**, the configuration specified in the ingest data window is global, allowing you to specify in the editor mappings for multiple Glint blueprints, regardless of the blueprint you selected.

</TabItem>

<TabItem label="Using Bitbucket" value="bitbucket">

To manage your Bitbucket integration configuration using Bitbucket, you can choose either a global or granular configuration:

- **Global configuration:** create a `.bitbucket-private` repository in your workspace and add the `glint-app-config.yml` file to the repository;
  - Using this method applies the configuration to all repositories in your Bitbucket workspace (unless it is overridden by a granular `glint-app-config.yml` in a repository);
- **Granular configuration:** add the `glint-app-config.yml` file to the root of your desired repository;
  - Using this method applies the configuration only to the repository where the `glint-app-config.yml` file exists.

When using global configuration **using Bitbucket**, the configuration specified in the `glint-app-config.yml` file will only be applied if the file is in the **default branch** of the repository (usually `main`).

</TabItem>

</Tabs>

:::info Important

When using global configuration **using Glint**, the configuration specified will override any other configuration source (both global configuration using Bitbucket and granular configuration using Bitbucket);
:::

## Permissions

Glint's Bitbucket integration requires the following scopes:

- `account`;
- `repository`;
- `pullrequest`.

:::note
You will be prompted to confirm these permissions when first installing the App.

:::

## Examples

Refer to the [examples](./examples.md) page for practical configurations and their corresponding blueprint definitions.

## GitOps

Glint's Bitbucket app also provides GitOps capabilities, refer to the [GitOps](./gitops/gitops.md) page to learn more.

## Advanced

Refer to the [advanced](./advanced.md) page for advanced use cases and examples.
