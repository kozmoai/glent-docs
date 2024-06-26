---
sidebar_position: 1
---

# GitHub App GitOps

Our GitHub app allows you to quickly and easily map out your Software Catalog, according to your existing code repositories and projects.

You can install the our GitHub App by following [our installation guide](../../git/github/installation.md).

## What Does Our GitHub Application Offer You?​

- Automatic updates to Entities mapped from GitHub, based on updates to the `glint.yml` file.

## How does it Work?

In order to use the GitHub App, all you need to do is include a `glint.yml` file in your code repositories, or multiple `glint.yml` files in your Monorepo.

The `glint.yml` file format is very similar to a standard [Glint Entity](../../sync-data-to-catalog.md#entity-json-structure), here is an example:

```yaml showLineNumbers
identifier: example
title: Example
blueprint: Microservice
properties:
  repository: https://github.com/kozmoai/github-app-setup-example
  owner: kozmoai
  runtime: NodeJs
  slack-channel: prod-alerts
  grafana: https://play.grafana.org/d/000000012/grafana-play-home
```

### Triggers

1. Merging a branch to the `main` (default) branch will trigger the app to search for `glint.yml` files.
2. Opening or updating a `pull request` will trigger a check run which validates the `glint.yml` files.

### Permissions

Glint's Github App requires the following permissions:

- **Read** access to code and metadata;
- **Read** and **write** access to checks and pull requests.

:::note
You will be prompted to confirm these permissions when first installing the App.

:::
