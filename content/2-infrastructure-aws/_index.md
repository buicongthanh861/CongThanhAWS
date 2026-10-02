---
title: "Infrastructure & AWS"
weight: 2
chapter: false
pre: "<b>2. </b>"
---

## AWS infrastructure with Terraform

Infrastructure is defined with **Terraform**, keeping configuration versioned, reviewable before applying changes, and consistent across deployments.

- Design a **multi-AZ VPC** with separate **Public Subnets** and **Private Subnets**.
- Configure **NAT Gateway**, routing, and **Security Groups** to control network traffic.
- Use CIDR `10.0.0.0/16` in `ap-southeast-1`, with subnets in `ap-southeast-1a` and `ap-southeast-1b`.
- Provision the **staging-demo-eks** cluster and a managed `general` node group using `t3.medium`, with 2 default nodes and scaling up to 5.
- Configure IAM roles and policies for EKS, worker nodes, ECR read access, and add-ons; integrate OIDC/IRSA.
- Install AWS Load Balancer Controller and EBS CSI driver, and configure a default StorageClass.

## Outcome

The default environment is `staging` in `ap-southeast-1`; the Kubernetes version and some environment values are defined in `terraform/locals.tf`. Reported provisioning time is **under 15 minutes**.

> Operational note: review `terraform plan` before applying; the current configuration uses one NAT Gateway. The database `values.yaml` declares `gp3`, but the StatefulSet template hardcodes `gp2`; align them if the StorageClass should be configurable through values.
