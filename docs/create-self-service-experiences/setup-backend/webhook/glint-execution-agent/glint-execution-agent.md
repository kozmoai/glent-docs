---
sidebar_position: 1
---

# Glint execution agent

Our execution agent provides you with a secure and convenient way to listen and act on invocations of Self-Service Actions and changes in the software catalog.

By using the execution agent, you don't need to expose a public endpoint for Glint to contact.

:::tip Public repository
Our Glint agent is open source - see it [**here**](https://github.com/kozmoai/glint-agent)
:::

:::note
To use the execution agent, please contact us via Intercom to receive a dedicated Kafka topic.
:::

The data flow when using the Glint execution agent is as follows:

- A new Self-Service Action or change in the software catalog is invoked;
- Glint sends the invocation event to your dedicated Kafka topic;
- The execution agent pulls the new invocation event from your Kafka topic, and sends it to the URL you specified.

![Glint Agent Architecture](/img/self-service-actions/glintExecutionAgentArchitecture.png)

## Next steps

- [Explore How to install and use the agent](/create-self-service-experiences/setup-backend/webhook/glint-execution-agent/installation-methods/helm.md)
- [Control the payload sent to your endpoint](/create-self-service-experiences/setup-backend/webhook/glint-execution-agent/control-the-payload.md)
