---
title: "CI/CD"
weight: 4
chapter: false
pre: "<b>4. </b>"
---

## Workflow overview

The GitHub Actions workflow is named CICD QA Workflow. It runs on pushes to the qa branch when changes affect client, server, or the workflow file. Its purpose is to check the source, package the two application images, push them to ECR, and update GitOps configuration in the IaC repository.

## Pipeline stages

1. GitLeaks scans the repository for secrets and saves a report artifact.
2. Checkov scans Terraform, Kubernetes manifests, and Dockerfiles. JSON results are saved as artifacts.
3. Trivy scans the client and server filesystems, focusing on HIGH and CRITICAL vulnerabilities; scan results are retained.
4. Lint and test commands are invoked for client and server when the packages define the corresponding scripts.
5. The frontend is built when the package defines a build script.
6. Docker builds the backend and frontend images, tagging them with the commit SHA and latest.
7. AWS credentials are used to log in to ECR and push both images.
8. The IaC update job uses YQ to update image tags in k8s/frontend/values.yaml and k8s/backend/values.yaml.
9. The Helm values change is committed and pushed to the master branch of the IaC repository.
10. Argo CD detects the new commit on master and syncs workloads to EKS.

## GitHub Actions access to AWS and IaC

The workflow requires these GitHub Actions secrets:

1. AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY for AWS authentication.
2. AWS_REGION to select the region containing ECR.
3. IAC_REPO_PAT to check out and push changes to the IaC repository.
4. SONARQUBE_TOKEN only if the currently commented SonarCloud job is enabled.

The following ECR repositories must exist before images can be pushed: 3-tier-user-platform-backend and 3-tier-user-platform-frontend.

## Current quality gates

Lint and test commands use the if-present option. The application packages currently do not define lint or test scripts, so GitHub Actions skips these steps without failing. Add a test runner, test cases, and package scripts before describing the pipeline as having verified unit tests.

Checkov commands currently allow the workflow to continue when the scanner returns an error. Scan reports can be retained for review, but Checkov is not currently a required quality gate.

SonarCloud is configured for the project, but its scan job is commented out. Enabling it requires a token, test coverage, and uncommenting the job after reviewing the workflow.

## Outcome

Based on the project results provided, release time decreased from around 30 minutes to approximately 5 minutes. The pipeline standardizes the path from an application commit to an image traceable by commit SHA, a GitOps repository update, and an Argo CD deployment.

The timing is a reported project result. Measure actual workflow runs to establish a comparable benchmark.
