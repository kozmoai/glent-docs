---
sidebar_position: 3
---

# Self Hosted Installation

:::note Prerequisites

- A registered organization in Glint;
- Your Glint user role is set to `Admin`.

:::

In organizations that have a self-hosted GitHub installation there is no access to our official public app, therefore there are some extra steps you need to take for installation:

1. Register Glint's GitHub App in your GitHub organization;
2. Deploy Glint's GitHub App Docker Image inside your VPC;
3. Install Glint's GitHub App in your GitHub organization and on select repositories.

## Registering Glint's GitHub App

1. First, navigate to your organization inside your self-hosted GitHub and click on Settings

![Org view](../../../../../static/img/integrations/github-app/SelfHostedOrganizaionView.png)

2. Inside the settings view, click on Developer Settings -> and then select GitHub Apps

![Settings view](../../../../../static/img/integrations/github-app/SelfHostedOrganizationSettings.png)

3. Click on "New GitHub App"

![New GitHub App](../../../../../static/img/integrations/github-app/SelfHostedNewGitHubApp.png)

4. Now insert all the following properties:

- GitHub App name: glint.io
- Homepage URL: https://useglint.io
- Webhook URL: HTTP Server URL, if you don't yet know the value of this step, leave it blank until you deploy the GitHub backend
- Webhook secret: Webhook secret (Any string you would like)
- Repository Permissions:
  - Actions: Read and Write (for executing self-service action using GitHub workflow)
  - Checks: Read and Write (for validating `Glint.yml`)
  - Contents: Readonly
  - Metadata: Readonly
  - Issues: Readonly
  - Pull Request: Read and Write
- Repository Events (required to receive webhook changes from GitHub):
  - Issues
  - Pull Request
  - Push
  - Workflow Run

Then select "Create GitHub App"

5. Now go to the settings of the created GitHub App and Generate a private key

![Generate Private key](../../../../../static/img/integrations/github-app/SelfHosetdGeneratePrivayKey.png)

And save the downloaded file

## Deployment

:::note Prerequisites

- You will need your Glint `CLIENT_ID` and `CLIENT_SECRET`. To find your Glint API credentials go to [Glint](https://app.useglint.io), hover on the `3 dots button` at the top right corner, and select `Credentials` then you will be able to view and copy your `CLIENT_ID` and `CLIENT_SECRET`:

<center>

![Glint Developer Portal Credentials Modal](../../../../../static/img/software-catalog/credentials-modal.png)

</center>

- In order to make use of [Self-Service Actions using GitHub Workflow](../../../../self-service-actions/github-workflow/github-workflow.md), please contact us at support@useglint.io.

:::

## Docker

To use our GitHub app you will need to deploy our official GitHub app docker image on your VPC.

It can be deployed on any platform that allows deploying images as containers such as: K8S, ECS, AWS App Runner, etc.

You can pull the Docker image by running:

```bash showLineNumbers
docker pull ghcr.io/kozmoai/glint-self-hosted-github-app:0.6.0
```

And now run it via:

```bash showLineNumbers
docker run \
  -e APP_ID=<APP_ID from register step> \
  -e WEBHOOK_SECRET=<WEBHOOK_SECRET from previous step> \
  -e GHE_HOST=<GITHUB BASE HOST, ie github.compay.com> \
  -e GLINT=<Any GLINT> \
  -e GLINT_URL=https://api.useglint.io \
  -e GLINT_CLIENT_ID=<CLIENT_ID> \
  -e GLINT_CLIENT_SECRET=<CLIENT_SECRET> \
  -e PRIVATE_KEY=<BASE 64 PRIVATEKEY> \
  ghcr.io/kozmoai/glint-self-hosted-github-app
```

| Env variable         | Description                                                                                           |
| -------------------- | ----------------------------------------------------------------------------------------------------- |
| `APP_ID`             | Application ID, it should show up in the edit GitHub App page in the upper section                    |
| `WEBHOOK_SECRET`     | The same string that was been used to register the application in the previous step                   |
| `GHE_HOST`           | Your organization's self-hosted GitHub hostname                                                       |
| `GLINT`               | The glint that the GitHub App will listen to                                                           |
| `GLINT_URL`           | Glint's API Base URL                                                                                   |
| `GLINT_CLIENT_ID`     | Glint client id for interacting with the API                                                           |
| `GLINT_CLIENT_SECRET` | Glint client secret for interacting with the API                                                       |
| `PRIVATE_KEY`        | A base64 encoded private key. Recieve it by running the command `base64 -i private-key-file-path.pem` |

## Health check route

A health check is a route that is used to check the health of a service. It is a means to ensure that the service is running properly and can perform its intended function.

Our GitHub App image previews such routes at `https://host:glint/health` for you to be able to monitor its status.

## Installing Glint's GitHub application

After you have the App registered in your organization and the docker is up and running, you can install the app and select the repositories to integrate it with

1. First, navigate to your organization inside your self-hosted GitHub and click on Settings

![Org view](../../../../../static/img/integrations/github-app/SelfHostedOrganizaionView.png)

2. Inside the settings view, click on Developer Settings -> and then select GitHub Apps

![Settings view](../../../../../static/img/integrations/github-app/SelfHostedOrganizationSettings.png)

3. Click `edit` on the GitHub app created at the step before

![GitHub app installation page](../../../../../static/img/integrations/github-app/SelfHostedEditGitHubApp.png)

4. Go to Install App -> and select the installation button on your wanted organization

5. Choose the repositories you want the app to be installed for.

![GitHub app installation chooses repositories](../../../../../static/img/integrations/github-app/SelfHostedInstallationRepoSelection.png)

## Next Steps

Now that you have the GitHub app installed, you can refer to our examples for some practical use cases:

- [Exporting pull requests as entities to Glint](./exporting-pull-requests)
- [Exporting repositories files and metadata as entities to Glint](./exporting-repositories.md)
- [Exporting entities via glint.yml definition file](../../gitops/github-app-gitops/quickstart.md)
