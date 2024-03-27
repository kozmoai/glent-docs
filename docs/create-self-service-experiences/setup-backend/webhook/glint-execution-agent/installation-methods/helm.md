---
sidebar_position: 1
---

# Helm

This page will walk you through the installation of the Glint execution agent in your Kubernetes cluster using Helm.

:::info
You can observe the helm chart and the available parameters [here](https://github.com/kozmoai/helm-charts/tree/main/charts/glint-agent).
:::

:::note prerequisites

- [Helm](https://helm.sh) must be installed to use the chart. Please refer to the Helm [documentation](https://helm.sh/docs/intro/install/) for further details about the installation.
- The connection credentials to Kafka are provided to you by Glint.
- If you want to trigger a GitLab Pipeline, you need to have a [GitLab trigger token](https://docs.gitlab.com/ee/ci/triggers/)

:::

## Installation

1. Add Glint's Helm repo by using the following command:

```bash
helm repo add kozmoai https://kozmoai.github.io/helm-charts
helm repo update
```

You can then run `helm search repo kozmoai` to see the charts.

2. Install the `glint-agent` chart by using the following command:


:::note
Remember to replace the placeholders for `YOUR_ORG_ID`, `YOUR_KAFKA_CONSUMER_GROUP`, `YOUR_GLINT_CLIENT_ID` and `YOUR_GLINT_CLIENT_SECRET`.
:::

```bash showLineNumbers
helm install my-glint-agent kozmoai/glint-agent \
    --create-namespace --namespace glint-agent \
    --set env.normal.GLINT_ORG_ID=YOUR_ORG_ID \
    --set env.normal.KAFKA_CONSUMER_GROUP_ID=YOUR_KAFKA_CONSUMER_GROUP \
    --set env.secret.GLINT_CLIENT_ID=YOUR_GLINT_CLIENT_ID \
    --set env.secret.GLINT_CLIENT_SECRET=YOUR_GLINT_CLIENT_SECRET
```

## Next Steps

- Refer to the [usage guide](/create-self-service-experiences/setup-backend/webhook/glint-execution-agent/usage.md) to set up a self-service action that sends a webhook.
- Customize the [payload mapping](/create-self-service-experiences/setup-backend/webhook/glint-execution-agent/control-the-payload.md?installationMethod=helm) to control the payload sent to the target.