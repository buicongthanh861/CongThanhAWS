---
title: "Project overview"
weight: 1
chapter: false
pre: "<b>1. </b>"
---

## Project objective

3-Tier User Platform is a user-management application deployed with a three-tier architecture. The project demonstrates application containerization, AWS infrastructure automation, and a repeatable delivery workflow based on CI/CD and GitOps.

The application consists of a React frontend, a Node.js and Express REST API, and a MySQL database. Terraform provisions the AWS network and Amazon EKS. Kubernetes workloads are described with Helm charts. GitHub Actions builds and releases images, while Argo CD reconciles deployment configuration from Git.

## Application architecture

### Presentation tier

The frontend uses React 17, React DOM, Axios, and Webpack 5. Users can view the user list and submit a new user through the interface. The frontend calls the REST API using the application API path.

The frontend has its own Dockerfile and is served by Nginx. Its image is pushed to Amazon ECR for Kubernetes to pull during deployment.

### Application tier

The backend runs on Node.js 18 or later and uses Express 4, MySQL2, and CORS. It listens on port 5000 by default and provides these operations:

1. GET /api/users to retrieve users.
2. POST /api/users to create a user.
3. PUT /api/users/:id to update a user.
4. DELETE /api/users/:id to delete a user.

Database connection settings are supplied through environment variables. On startup, after a successful connection, the application creates the users table if it does not already exist.

### Data tier

MySQL stores the users table with id, name, email, and role fields. Email is unique, and role supports Admin and User. On EKS, MySQL runs as a StatefulSet and uses a PersistentVolumeClaim to retain data across Pod lifecycles.

## Request flow

1. A user sends a request to the NGINX Ingress Controller.
2. Ingress routes website requests to the frontend service.
3. API requests under /api are routed to the backend service.
4. The backend reads or writes user records in MySQL.
5. MySQL stores data on a volume provisioned through Kubernetes storage.

The frontend and backend run in separate namespaces. The database also has its own namespace to separate its resources and configuration.

## Release flow

1. A developer pushes application changes to the qa branch.
2. GitHub Actions runs security checks and runs lint or test commands only when the corresponding package scripts exist.
3. The pipeline builds frontend and backend images and pushes them to ECR.
4. The pipeline uses YQ to update image tags in the Helm values in the IaC repository.
5. The pipeline commits the change to the master branch of the IaC repository.
6. Argo CD detects the Git change and reconciles the application on EKS.

## Technology components

1. AWS: Amazon EKS, Amazon ECR, VPC, IAM, NAT Gateway, and Security Groups.
2. Infrastructure as Code: Terraform.
3. Containers and deployment: Docker, Kubernetes, Helm, and NGINX Ingress Controller.
4. GitOps: Argo CD.
5. CI/CD: GitHub Actions and YQ.
6. Security checks: GitLeaks, Checkov, and Trivy.
7. Optional monitoring stack: Prometheus, Grafana, and Alertmanager.

## Current scope and status

The frontend currently supports viewing and adding users. The backend also provides update and delete APIs, but the Edit and Delete controls in the interface are not connected to those APIs yet.

The workflow runs lint and test commands only if the package scripts exist. Those scripts are not currently defined in the packages, so the current pipeline should not be described as running unit tests.

Terraform does not currently install the monitoring stack automatically. Prometheus, Grafana, and Alertmanager must be installed separately before using the port-forward scripts.
