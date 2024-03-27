---
title: Security
---

# Security

This page includes additional configurations required to allow Glint to interact with your [backend](../setup-backend/setup-backend.md) when using self-service actions.

## Glint IP addresses for allow lists

When Glint makes outbound calls (for example when using the [Webhook](../setup-backend/webhook/webhook.md) invocation method), the source IP addresses are static.

Glint outbound calls will originate from one of the following IP addresses:

```text showLineNumbers
44.221.30.248, 44.193.148.179, 34.197.132.205, 3.251.12.205
```
