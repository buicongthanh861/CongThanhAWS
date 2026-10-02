---
title: "Infrastructure & AWS"
weight: 2
chapter: false
pre: "<b>2. </b>"
---

## Infrastructure objective

The 3-tier-user-platform-iac repository manages AWS resources and Kubernetes add-ons with Terraform. Infrastructure as Code keeps configuration in Git, allows changes to be reviewed with plan, and makes environments reproducible from configuration files.

## VPC networking

1. The VPC uses the 10.0.0.0/16 address range.
2. The environment is deployed in the ap-southeast-1 region.
3. Subnets are placed in the ap-southeast-1a and ap-southeast-1b Availability Zones.
4. Public subnets provide entry points for components that require external access according to the network configuration.
5. Private subnets are intended for nodes and workloads that do not require direct public addressing.
6. An Internet Gateway, NAT Gateway, Elastic IP, route tables, and Security Groups provide connectivity between network layers.

The current configuration uses one NAT Gateway. Availability and cost should be reviewed for each Availability Zone before production use.

## Amazon EKS

The cluster is named staging-demo-eks. The default environment is staging. The Kubernetes version is configured in terraform/locals.tf.

The managed node group is named general and uses t3.medium instances. It has two default nodes and can scale to a maximum of five nodes. Actual scaling depends on AWS quotas, application load, and autoscaling configuration.

## IAM and access

Terraform defines IAM roles and policies for the EKS control plane, worker nodes, ECR image read access, and required add-ons. An OIDC provider allows Kubernetes service accounts to assume IAM roles through IRSA instead of granting every workload broad permissions through the worker node role.

Review IAM permissions using least privilege. Avoid long-lived access keys in applications when IAM roles can be used instead.

## Kubernetes add-ons

Terraform installs Argo CD in the argocd namespace. The Argo CD server is configured to run insecure behind a LoadBalancer. Restrict access and configure TLS at the ingress or load balancer layer before exposing it publicly.

Other add-ons include AWS Load Balancer Controller for managing load balancer resources and the EBS CSI driver for provisioning PersistentVolumes. The Kubernetes configuration sets gp2 as the default StorageClass.

## Infrastructure repository structure

1. The terraform directory contains AWS infrastructure and Kubernetes add-ons.
2. The argocd/applications directory contains Argo CD Application manifests.
3. The k8s/frontend directory contains the frontend Helm chart.
4. The k8s/backend directory contains the backend Helm chart.
5. The k8s/database directory contains the MySQL Helm chart.
6. The scripts directory contains commands for cluster access, ingress installation, and port forwarding.

## Provisioning steps

1. Install AWS CLI, Terraform, kubectl, and Helm.
2. Configure AWS credentials with permission to create the project resources.
3. Change to the terraform directory and run terraform init to initialize providers and modules.
4. Run terraform validate to check the configuration syntax.
5. Run terraform plan and review the resources to be created or changed.
6. Run terraform apply after reviewing the plan.
7. Use scripts/connect.sh to update kubeconfig and check the cluster nodes.

## Outcome and operational notes

Provisioning time is reported as under 15 minutes. Actual timing depends on AWS service conditions, quotas, region, and the resources being created.

The database chart has a configuration mismatch: values.yaml declares gp3, while the StatefulSet template currently hardcodes gp2. Review the template and StorageClass before deployment so the declared value is actually used.

Terraform state can contain sensitive information. Do not commit state files, secret-bearing tfvars files, or credentials. Configure a remote backend with encryption and state locking before collaborating.

Do not run terraform destroy without confirming the impact. It can delete the cluster, node group, network, and AWS resources managed by Terraform.
