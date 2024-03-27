---
sidebar_position: 4
---

import Tabs from "@theme/Tabs"
import TabItem from "@theme/TabItem"
import DeleteDependents from '../../../../generalTemplates/\_delete_dependents_git_explanation_template.md'

# Advanced

The Bitbucket integration supports additional flags to provide additional configuration, making it easier to configure its behavior to your liking.

## Using advanced configurations

The following advanced configuration parameters are available:

<Tabs groupId="config" queryString="parameter">

<TabItem label="Spec path" value="specPath">

The `specPath` parameter specifies a string that Glint's Bitbucket app will use when constructing search paths leading to a `yml` file, every path in the repository which ends with the `specPath` value will be scanned.

- Default value: `glint.yml`
- Use case:
  - If you want the app to scan a different file than `glint.yml` (for example, change configure the app to scan files named `my-glint-config.yml` using the pattern `my-glint-config.yml`);
  - If you want the app to ignore `glint.yml` files in certain paths.

</TabItem>

<TabItem label="Delete dependent entities" value="deleteDependent">

<DeleteDependents/>

- Default: `false` (disabled)
- Use case: Deletion of dependent Glint entities. Must be enabled, if you want to delete a target entity (and its source entities) in a required relation.

</TabItem>

<TabItem label="Enable merge entity" value="enableMergeEntity">

The `enableMergeEntity` parameter specifies whether to use the [create/update](/build-your-software-catalog/custom-integration/api?operation=create-update#usage) or [create/override](/build-your-software-catalog/custom-integration/api?operation=create-override#usage) strategy when creating entities listed in a `glint.yml` file.

- Default value: `true` (use create/update)
- Use case: use `false` if you want Bitbucket to be the source-of-truth for catalog entities. Use `true` if you want to use Bitbucket as the source for some properties of entities in the catalog, and use other sources to for properties which are subject to change automatically.

</TabItem>

</Tabs>

All of the advanced configurations listed below can be added to the [`glint-app-config.yml`](./bitbucket.md#glint-app-configyml-file) file.
