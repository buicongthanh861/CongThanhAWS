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
- Provision **Amazon EKS** and the **IAM Roles** required by the cluster and workloads.
- Organize Terraform into **reusable modules** to standardize environment provisioning.

## Outcome

Provisioning time was reduced from several hours to **under 15 minutes**, based on the project results provided. Infrastructure can be deployed repeatedly from the configuration maintained in the IaC repository.

> Performance figures reflect the project results provided; actual timing can vary with AWS configuration and service conditions.
