---
title: "DevOps / Cloud Engineer"
weight: 1
chapter: false
---
# 3-Tier User Platform on AWS

Hi, I am **buicongthanh861**. This is the project report for **3-Tier User Platform**, covering how a user-management application is deployed on AWS and how its infrastructure, CI/CD, and GitOps delivery are automated.

## Project overview

The application consists of a React frontend, a Node.js/Express REST API, and MySQL. Kubernetes infrastructure is provisioned on Amazon EKS with Terraform. GitHub Actions checks the source, builds and pushes images to ECR, and updates Helm values in the IaC repository; Argo CD reconciles those changes to the cluster.

**Tech stack:** AWS EKS, ECR, VPC, IAM, Terraform, Kubernetes, Helm, Argo CD, GitHub Actions, Prometheus, Grafana, Trivy, Checkov, GitLeaks.

## Contents

1. [Project overview](1-project-overview/)
2. [Infrastructure & AWS](2-infrastructure-aws/)
3. [Kubernetes & GitOps](3-kubernetes-gitops/)
4. [CI/CD](4-cicd/)
5. [DevSecOps & Monitoring](5-devsecops-monitoring/)
6. [Repositories & outcomes](6-repositories-results/)
