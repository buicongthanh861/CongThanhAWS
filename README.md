# 3-Tier AWS Platform — DevOps / Cloud Engineer

This repository contains the bilingual project report for **buicongthanh861**. It documents a three-tier user platform deployed on AWS with Terraform, Amazon EKS, Kubernetes, Helm, Argo CD, and GitHub Actions.

## Project repositories

- Application: [3-tier-user-platform](https://github.com/buicongthanh861/3-tier-user-platform/tree/qa) (`qa` branch)
- Infrastructure: [3-tier-user-platform-iac](https://github.com/buicongthanh861/3-tier-user-platform-iac)

## Report contents

The Hugo site covers AWS infrastructure, Kubernetes and GitOps, CI/CD, DevSecOps, monitoring, and project outcomes in English and Vietnamese.

## Run locally

Install [Hugo](https://gohugo.io/installation/), then run:

```powershell
hugo server
```

Open <http://localhost:1313> to preview the site. The GitHub Actions workflow builds and deploys the site to GitHub Pages when changes are pushed to `main`.
