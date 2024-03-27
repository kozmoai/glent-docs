---
sidebar_position: 3
---

import Tabs from "@theme/Tabs"
import TabItem from "@theme/TabItem"
import GlintYmlStructure from '../../\_glint_yml_gitops_structure_template.md'
import BasicFileProperties from '../../\_basic_file_properties_template.md'
import RelativeFileProperties from '../../\_relative_file_properties_template.md'
import GitOpsPushEvent from '../../\_git_gitops_push_events_explanation.mdx'

# GitOps

Glint's GitHub integration makes it possible to manage Glint entities with a GitOps approach, making your code repositories the source of truth for the various infrastructure assets you want to manage.

## 💡 GitHub GitOps common use cases

- Use GitHub as the source-of-truth for your **microservices**, **packages**, **libraries** and other software catalog assets.
- Allow developers to keep the catalog up-to-date, by making updates to files in their Git repositories.
- Create a standardized way to document software catalog assets in your organization.

## Managing entities using GitOps

To manage entities using GitOps, you will need to add a `glint.yml` file to the **default branch** (usually `main`) of your repository.

The `glint.yml` file can specify one or more Glint entities that will be ingested to Glint, and any change made to the `glint.yml` file will also be reflected inside Glint.

:::tip Github app
To manage entities using GitOps and the `glint.yml` file, Glint's [Github app](/build-your-software-catalog/sync-data-to-catalog/git/github/installation/) must be installed, as it listens to `push` events sent from Github.

This means that if the `glint.yml` file exists in the repository before installing the app, it will not be picked up automatically. You will need to make some update to the `glint.yml` file and push it to the repository in order for the Git app to properly track and ingest the entity information.
:::

### GitOps `glint.yml` file

The `glint.yml` file is how you specify your Glint entities that are managed using GitOps and whose data is ingested from your Git repositories.

Here are examples for valid `glint.yml` files:

<Tabs groupId="format">

<TabItem value="single" label="Single entity">

```yaml showLineNumbers
identifier: myEntity
title: My Entity
blueprint: myBlueprint
properties:
  myStringProp: myValue
  myNumberProp: 5
  myUrlProp: https://example.com
relations:
  mySingleRelation: myTargetEntity
  myManyRelation:
    - myTargetEntity1
    - myTargetEntity2
```

</TabItem>

<TabItem value="multiple" label="Multiple entities">

```yaml showLineNumbers
- identifier: myEntity1
  title: My Entity1
  blueprint: myFirstBlueprint
  properties:
    myStringProp: myValue
    myNumberProp: 5
    myUrlProp: https://example.com
  relations:
    mySingleRelation: myTargetEntity
    myManyRelation:
      - myTargetEntity1
      - myTargetEntity2
- identifier: myEntity
  title: My Entity2
  blueprint: mySecondBlueprint
  properties:
    myStringProp: myValue
    myNumberProp: 5
    myUrlProp: https://example.com
```

</TabItem>

</Tabs>

Since both of the valid `glint.yml` formats follow the same structure, the following section will explain the format based on the single entity example.

### `glint.yml` structure

<GlintYmlStructure/>

### Ingesting repository file contents

<BasicFileProperties/>

#### Using relative paths

<RelativeFileProperties/>

## Examples

Check out the [example repository](https://github.com/kozmoai/github-app-setup-example) for a microservice blueprint and a matching `glint.yml` file which specifies a microservice entity.

## Advanced

Refer to the [advanced](../advanced.md) page for advanced use cases and configurations.
