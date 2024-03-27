---
sidebar_position: 6
---

# GitOps using Glint CRDs

You can use GitOps, [Glint's K8s exporter](/build-your-software-catalog/sync-data-to-catalog/kubernetes/kubernetes.md) and [Glint's Entity CRDs](/build-your-software-catalog/sync-data-to-catalog/kubernetes/glint-crd.md) to export custom entities in to Glint.

:::note
For fully understanding how to use GitOps with Glint's CRDs to map entities, make sure you are familiar with:

- [Glints Kubernetes exporter](/build-your-software-catalog/sync-data-to-catalog/kubernetes/kubernetes.md)
- [Glints Entity CRDs](/build-your-software-catalog/sync-data-to-catalog/kubernetes/glint-crd.md)

:::

## Export entities with GitOps and Kubernetes use cases

- Use your K8s clusters as the source-of-truth for your **microservices**, **packages**, **libraries** and other software catalog assets;
- Update Glint in a "Push Only" method, where no elevated permissions are required for Glint to interact with your infrastructure;
- Allow developers to keep the catalog up-to-date, by making updates to Kubernetes manifest files in their Git repositories;
- Create a standardized way to document software catalog assets in your organization;
- etc.

## Managing entities defined using CRDs and GitOps

Glint's CRDs allow defining and mapping any kind of entity using Kubernetes. You can find an example for doing this in the [Glint CRDs - mapping a microservice example](/build-your-software-catalog/sync-data-to-catalog/kubernetes/glint-crd.md#example---mapping-a-microservice-using-glint-crds).
Mapping entities can be done using any Continuous Deployment (CD) solution, for example ArgoCD or FluxCD, by deploying the Glint custom resources using the CD solution, and mapping their definition to Glint using Glint's K8s exporter.

To achieve this, follow these general steps for any CD solution:

1. Navigate to [Glint's CRDs document](/build-your-software-catalog/sync-data-to-catalog/kubernetes/glint-crd.md#deploying-ports-crds) page to learn how to deploy the CRDs. You can deploy them as part of your CD pipeline by placing the CRD manifests in one of your CD source directory/applications;
2. Define a Kubernetes Glint entity manifest using Glint's CRD, which contains the data model and data you wish to map to Glint, and deploy it to your kubernetes cluster using your CD solution;
3. [Update](/build-your-software-catalog/sync-data-to-catalog/kubernetes/kubernetes.md#updating-exporter-configuration) Glint's K8s exporter resource mapping to map the Glint CRD you just created.

The entities defined using Glint's CRD will appear in your Glint environment.
