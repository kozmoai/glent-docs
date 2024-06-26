---
sidebar_position: 4
title: Exporting repositories
---

:::note Prerequisites

- [Please install our Bitbucket app](./installation).

:::

# Exporting repositories

In this tutorial you will learn how to create a `microservice` Blueprint that contains the properties `repository URL`, `Workspace` and `Project`, which are automatically synced from Bitbucket to Glint Entity.

1. Create a `microservice` Blueprint and `glint-app-config.yml` configuration file.

To export your Bitbucket `repositories` to Glint, you can use the following Glint Blueprints definitions, and `glint-app-config.yml`:

<details>
<summary> Microservice Blueprint </summary>

```json showLineNumbers
{
  "identifier": "microservice",
  "title": "Microservice",
  "icon": "Service",
  "schema": {
    "properties": {
      "url": {
        "title": "URL",
        "format": "url",
        "type": "string"
      },
      "project": {
        "type": "string",
        "title": "Project"
      }
    },
    "required": []
  },
  "mirrorProperties": {},
  "calculationProperties": {},
  "relations": {}
}
```

</details>

Place the `glint-app-config.yml` in the repository's root folder.

<details>

<summary> Glint glint-app-config.yml </summary>

```yaml showLineNumbers
resources:
  - kind: repository
    selector:
      query: "true" # JQ boolean query. If evaluated to false - skip syncing the object.
    glint:
      entity:
        mappings:
          identifier: ".name" # The Entity identifier will be the repository name. After the Entity is created, the exporter will send `PATCH` requests to update this microservice within Glint.
          title: ".name"
          blueprint: '"microservice"'
          properties:
            url: ".links.html.href" # fetch the repository URL from the Bitbucket metadata and inject it as a URL property.
            project: ".project.name" # fetch the project from the Bitbucket metadata and inject it as a property.
```

</details>

:::info

- We leverage [JQ JSON processor](https://stedolan.github.io/jq/manual/) to map and transform Bitbucket objects to Glint Entities.
- Click [Here](https://developer.atlassian.com/cloud/bitbucket/rest/api-group-repositories/#api-repositories-workspace-repo-slug-get) for the Bitbucket repository object structure.

:::

2. Push `glint-app-config.yml` to your default branch.

That's it! after the push is complete, the exporter will start ingesting the Entities on the next commit to the repository.

![Developer Portal Microservice](../../../../../static/img/integrations/bitbucket-app/BitbucketMicroservices.png)
