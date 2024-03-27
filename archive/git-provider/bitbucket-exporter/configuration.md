---
sidebar_position: 3
---

# Configuration

## How to configure the Bitbucket app?

There are 2 methods to configure the Bitbucket app:

1. For a `single repository`: create a `glint-app-config.yml` file in it;
2. For `all repositories` in the organization: create a `.bitbucket-private` repository in the organization and create a `glint-app-config.yml` file in it.

### Example `glint-app-config.yml`

```yaml showLineNumbers
resources:
  - kind: pull-request
    selector:
      query: "true"
    glint:
      entity:
        mappings:
          identifier: ".destination.repository.name + (.id|tostring)"
          title: ".title"
          blueprint: '"pullRequest"'
          properties:
            creator: ".author.display_name"
            assignees: "[.participants[].user.display_name]"
            reviewers: "[.reviewers[].user.display_name]"
            status: ".state"
            createdAt: ".created_on"
            updatedAt: ".updated_on"
            description: ".description"
            link: ".links.html.href"
```
