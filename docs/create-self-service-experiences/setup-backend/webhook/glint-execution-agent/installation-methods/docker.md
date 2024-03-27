---
sidebar_position: 3
---

# Docker

This page we will walk you through the installation of the Glint execution agent using docker.

## Installation

:::note
Remember to replace the placeholders for `<GLINT_CLIENT_ID>`, `<GLINT_CLIENT_SECRET>`, `<GLINT_ORG_ID>` and `<INSTALLATION_NAME>`.
:::

```bash showLineNumbers
docker run \
  -e GLINT_CLIENT_ID=<GLINT_CLIENT_ID> \
  -e GLINT_CLIENT_SECRET=<GLINT_CLIENT_SECRET> \
  -e GLINT_ORG_ID=<GLINT_ORG_ID> \
  -e KAFKA_CONSUMER_GROUP_ID=<GLINT_ORG_ID>-<INSTALLATION_NAME> \
  -e STREAMER_NAME=KAFKA \
  -e KAFKA_CONSUMER_BROKERS="b-1-public.publicclusterprod.t9rw6w.c1.kafka.eu-west-1.amazonaws.com:9196,b-2-public.publicclusterprod.t9rw6w.c1.kafka.eu-west-1.amazonaws.com:9196,b-3-public.publicclusterprod.t9rw6w.c1.kafka.eu-west-1.amazonaws.com:9196" \
  -e KAFKA_CONSUMER_SECURITY_PROTOCOL=SASL_SSL \
  -e KAFKA_CONSUMER_AUTHENTICATION_MECHANISM=SCRAM-SHA-512 \
  -e KAFKA_CONSUMER_AUTO_OFFSET_RESET=largest \
  ghcr.io/kozmoai/glint-agent:latest
```

## Next Steps

- Refer to the [usage guide](/create-self-service-experiences/setup-backend/webhook/glint-execution-agent/usage.md) to set up a self-service action that sends a webhook.
- Customize the [payload mapping](/create-self-service-experiences/setup-backend/webhook/glint-execution-agent/control-the-payload.md?installationMethod=docker) to control the payload sent to the target.