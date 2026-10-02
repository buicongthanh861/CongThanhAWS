---
title: "Kubernetes & GitOps"
weight: 3
chapter: false
pre: "<b>3. </b>"
---

## Package the application with Helm

The infrastructure repository contains three independent Helm charts under k8s. Each chart manages the configuration and resources for one application component.

### Frontend

The frontend runs as a Kubernetes Deployment and is exposed inside the cluster through a ClusterIP Service. Ingress uses the root path / for website requests. A Horizontal Pod Autoscaler is configured from one to three replicas based on CPU utilization.

### Backend

The backend runs as a Deployment with an internal Service on port 5000. Ingress accepts requests at /api and applies the rewrite configured in the manifest. The chart contains a ConfigMap and Secret for database connection settings. The HPA scales from one to three replicas based on CPU.

### Database

MySQL 8 runs as a StatefulSet with an internal Service. Connection credentials are stored in a Kubernetes Secret. A 5 GiB PersistentVolumeClaim stores data outside the Pod lifecycle.

## Ingress and application access

The NGINX Ingress Controller is installed with Helm through scripts/nginx.sh. The script configures HTTP NodePort 30080. Ingress routes / to the frontend and /api to the backend.

The frontend Dockerfile serves static content with Nginx. Verify the proxy and API path configuration during deployment because the Nginx in the frontend image does not automatically forward /api to the backend unless configured to do so.

## GitOps with Argo CD

Terraform installs Argo CD in the argocd namespace. Application manifests are stored under argocd/applications and track the IaC repository.

1. Argo CD tracks the master branch and the frontend, backend, and database chart directories.
2. Each application is installed into its own namespace with the same name as the component.
3. Automated sync applies changes from Git to the cluster.
4. Prune removes Kubernetes resources that are no longer declared in Git.
5. Self-heal restores resources changed manually to the state defined in Git.

Before deployment, confirm the repository URL, master branch, and chart path in every Application manifest. Incorrect values prevent Argo CD from reconciling the intended configuration.

## Operations scripts

1. scripts/connect.sh updates the EKS kubeconfig and checks the nodes.
2. scripts/nginx.sh installs the NGINX Ingress Controller with Helm.
3. scripts/argocd.sh forwards Argo CD to localhost port 8080 and prints the initial admin password.
4. scripts/grafana.sh forwards Grafana to localhost port 3000.
5. scripts/prometheus.sh forwards Prometheus to localhost port 9090.
6. scripts/start-all.sh updates kubeconfig, installs NGINX, and starts the port-forward processes in the background.

The scripts are written for Bash and use utilities such as /tmp and pkill. Run them in Linux, WSL, or a compatible Git Bash environment, and check execute permissions before use.

## Workload deployment sequence

1. Complete Terraform apply and confirm that EKS nodes are Ready.
2. Run the cluster connection script and install the NGINX Ingress Controller.
3. Apply the manifests under argocd/applications.
4. Check Argo CD Application status and wait for sync to complete.
5. Check Deployments, Pods, Services, PVCs, and Ingress resources in the frontend, backend, and database namespaces.

## Configuration to verify

The backend is configured to connect to the database Service in the database namespace, using the test_db database and the default MySQL port. Verify that the hostname, username, password, and database name in Helm values and Secrets match the MySQL configuration.

The frontend and backend define resource requests, limits, and CPU-based HPA at 50 percent. Ensure that a metrics provider is available in the cluster so the HPA can read metrics.

The database requires a 5 GiB PVC with ReadWriteOnce access mode. The values file declares gp3, but the StatefulSet template hardcodes gp2. Align these settings before relying on the chart value.
