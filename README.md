# GitOps Automated Kubernetes Deployment

This repository serves as the source of truth for an automated GitOps deployment pipeline using **ArgoCD** and **Kubernetes**. 

## Project Overview

In this project, any changes pushed to the `main` branch of this repository are automatically detected by ArgoCD, which then synchronizes those changes into the connected Kubernetes cluster. This ensures that the cluster's state always perfectly matches the declarative configuration defined in this repository.

## Repository Structure

- `deployment.yaml`: Defines the Kubernetes Deployment for our sample Nginx application. It specifies the container image, replica count, and necessary labels.
- `service.yaml`: Defines the Kubernetes Service (LoadBalancer) used to expose the Nginx pods to the network.
- `application.yaml`: The ArgoCD Application manifest. This is the core GitOps resource that instructs ArgoCD to watch this GitHub repository and deploy the manifests into the cluster.

## How to use this repository

### 1. Triggering an Automated Deployment
To see GitOps in action:
1. Modify the `deployment.yaml` file (e.g., change `replicas: 2` to `replicas: 5`).
2. Commit and push the changes to GitHub.
3. ArgoCD will automatically detect the drift and apply the changes to your Kubernetes cluster within ~3 minutes.

### 2. Local Cluster Setup (WSL & Minikube)
If you are running this locally using Minikube on WSL, use the following commands:

**Start the cluster:**
```bash
minikube start --memory=2000mb
```

**Access the application:**
```bash
minikube service sample-app-service
```
*(Use the `127.0.0.1` tunnel URL provided by Minikube).*

**Access the ArgoCD UI:**
```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
Login at `https://localhost:8080` (Username: `admin`).

Retrieve the initial password:
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
