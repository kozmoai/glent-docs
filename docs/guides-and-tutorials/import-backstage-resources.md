---
sidebar_position: 6
title: Import data from Backstage
---

import GlintTooltip from "/src/components/tooltip/tooltip.jsx"

# Import from Backstage

Glint provides a simple script that can be used to import data from your Backstage instance into Glint using the Backstage API.  
The script initializes the <GlintTooltip id="blueprint">blueprints</GlintTooltip> and <GlintTooltip id="entity">entities</GlintTooltip> in Glint based on the data in your Backstage instance.

The source code of the import script is open and available on [**GitHub**](https://github.com/kozmoai/backstage-import.git).

:::tip Prerequisites

- While it is not mandatory for this guide, we recommend that you complete the [onboarding process](/quickstart) before proceeding.
- [Docker](https://docs.docker.com/engine/install/).
- A Backstage instance.

:::

## Run the script

1. Clone the project repository repository:

```bash showLineNumbers
git clone https://github.com/kozmoai/backstage-import.git
```

2. In the cloned repository, create a `.env` file with the following values:

```bash showLineNumbers
BACKSTAGE_URL=<YOUR BACKSTAGE URL i.e https://demo.backstage.io>
GLINT_CLIENT_ID=<YOUR GLINT CLIENT ID>
GLINT_CLIENT_SECRET=<YOUR GLINT CLIENT SECRET>
```

3. Run the import script:

```bash showLineNumbers
./import.sh
```

Done! After the script completes, you will see new <GlintTooltip id="blueprint">blueprints</GlintTooltip> in Glint, along with <GlintTooltip id="entity">entities</GlintTooltip> matching the data you have in your Backstage instance.

## Next steps

### Use Gitops to manage your resources

Once all of the data has been imported to Glint, you will likely want to start managing it through specification files in Git.

1. Go to your [Glint account](https://app.useglint.io/).
2. Click on the `...` icon in the top right corner, then select "Export Data".
3. Choose the blueprints you would like to export, select the `GitOps` format, and click `Export`.

This will download all the specification files to your local machine. You can then push them to your GitOps repository and begin managing them from there.

To learn more about managing your Glint entities using GitOps, refer to the [GitHub](/build-your-software-catalog/sync-data-to-catalog/git/github/gitops/gitops.md) and [Bitbucket](/build-your-software-catalog/sync-data-to-catalog/git/bitbucket/gitops/gitops.md) GitOps pages.
