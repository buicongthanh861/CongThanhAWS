---
title: "Kubernetes & GitOps"
weight: 3
chapter: false
pre: "<b>3. </b>"
---

## Package the application with Helm

The IaC repository provides three independent Helm charts under `k8s/`:

- **Frontend**: Deployment, ClusterIP Service, `/` Ingress, and HPA from 1–3 replicas.
- **Backend**: Deployment, port `5000` Service, `/api` Ingress, HPA from 1–3 replicas, ConfigMap, and database Secret.
- **Database**: MySQL `8.0` StatefulSet, internal service, Secret, and `5Gi` PVC.

Frontend and backend ingress traffic is served through NGINX Ingress Controller, installed by an operations script with HTTP NodePort `30080`.

## Reconcile deployments with Argo CD

**Argo CD** implements GitOps by tracking declarative configuration in Git and reconciling the desired state with the Kubernetes cluster.

- Three Argo CD Applications deploy into separate `frontend`, `backend`, and `database` namespaces.
- Track the `master` branch with automated sync, pruning of resources removed from Git, and self-healing on cluster drift.
- Terraform installs Argo CD into the `argocd` namespace; its server runs insecure behind a LoadBalancer.

Ensure the repository URL and branch in `argocd/applications/` match the actual IaC repository.
