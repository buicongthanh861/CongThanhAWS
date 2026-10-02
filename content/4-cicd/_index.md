---
title: "CI/CD"
weight: 4
chapter: false
pre: "<b>4. </b>"
---

## GitHub Actions pipeline

The `CICD QA Workflow` runs on pushes to `qa` that change `client/`, `server/`, or the workflow file. Its main flow:

1. Scan secrets with **GitLeaks**; scan Terraform/Kubernetes/Dockerfiles with **Checkov** and client/server filesystems with **Trivy**.
2. Run lint/tests and build the client when the corresponding package scripts exist.
3. Build frontend/backend images and push them to **Amazon ECR** with commit SHA and `latest` tags.
4. Use **YQ** to update image tags in the Helm values in the IaC repository, then commit/push to `master`.
5. **Argo CD** tracks changes on `master` and syncs the charts to EKS.

Scan artifacts are retained by the workflow. Checkov commands currently use `|| true`, so scan findings do not fail the pipeline.

## Outcome

Release time is reported to have dropped from around 30 minutes to **approximately 5 minutes**. Note: the packages currently do not define `lint` or `test` scripts, so steps invoked with `--if-present` are skipped and the workflow does not yet run unit tests. The workflow requires AWS credentials, an `IAC_REPO_PAT` that can push to the IaC repository, and frontend/backend ECR repositories.
