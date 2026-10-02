---
title: "Repositories and outcomes"
weight: 6
chapter: false
pre: "<b>6. </b>"
---

## Project repositories

1. Application: [3-tier-user-platform, qa branch](https://github.com/buicongthanh861/3-tier-user-platform/tree/qa)
2. Infrastructure as Code: [3-tier-user-platform-iac](https://github.com/buicongthanh861/3-tier-user-platform-iac)

## Project outcomes

1. Built a three-tier user-management application with a React frontend, Node.js REST API, and MySQL database.
2. Defined AWS networking and Amazon EKS infrastructure with Terraform, making the environment reproducible from repository configuration.
3. Organized frontend, backend, and MySQL as separate Helm charts deployed in separate Kubernetes namespaces.
4. Automated synchronization from the IaC repository to EKS with Argo CD automated sync, prune, and self-heal.
5. Automated frontend and backend image builds and ECR pushes, using commit SHA tags for traceability.
6. Updated image tags in Helm values with YQ to trigger the GitOps deployment flow.
7. Reported provisioning time is under 15 minutes and reported release time is approximately 5 minutes.

The timing figures are project-reported results. Keep workflow run logs if measured evidence is required in a portfolio or submission.

## Current limitations

1. The frontend currently connects user listing and creation. Update and delete operations exist in the backend but are not complete in the user interface.
2. The packages do not define lint or test scripts. Steps invoked with if-present are currently skipped.
3. Checkov does not currently block the pipeline when findings are returned because the workflow allows the command to continue.
4. The monitoring stack is not installed automatically by Terraform. Alertmanager email configuration still needs to be added and tested.
5. The MySQL template hardcodes gp2 while the values file declares gp3.
6. Verify the repository URL and master branch in the Argo CD Application manifests before deployment.
7. Review demo credentials, IAM permissions, EKS endpoint access, TLS, and ingress exposure before production use.

## Future improvements

1. Add unit tests and lint scripts for the frontend and backend, then make these pipeline steps required.
2. Make Checkov a quality gate after reviewing and addressing applicable findings.
3. Install kube-prometheus-stack, create dashboards and alert rules, and configure an Alertmanager email receiver.
4. Align the MySQL chart StorageClass and test PVC provisioning on EKS.
5. Complete user update and delete operations in the frontend.
6. Add authentication, authorization, input validation, and environment-specific CORS restrictions.
7. Configure encrypted and locked Terraform remote state, and test MySQL backup and recovery.

## Summary

The project combines application development, Infrastructure as Code, containers, Kubernetes, CI/CD, GitOps, and security checks. Separating the application repository from the infrastructure repository allows image changes to be reviewed in Git and reconciled to EKS in a controlled way.
