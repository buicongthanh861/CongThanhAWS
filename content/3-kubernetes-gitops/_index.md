---
title: "Kubernetes & GitOps"
weight: 3
chapter: false
pre: "<b>3. </b>"
---

## Package the application with Helm

The application is organized into independent **Helm Charts** for:

- **Frontend**
- **Backend**
- **MySQL StatefulSet**

Separating the charts makes it easier to manage configuration, versions, and the deployment lifecycle of each component.

## Reconcile deployments with Argo CD

**Argo CD** implements GitOps by tracking declarative configuration in Git and reconciling the desired state with the Kubernetes cluster.

- Automatically sync configuration from the Git repository to **Amazon EKS**.
- Detect configuration drift between Git and the cluster.
- Self-heal resources changed outside the declared deployment workflow.

Git serves as the source of truth for deployment configuration, with change history and reviewability.
