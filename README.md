
# Saleor Platform Deployment on Kubernetes

This repository contains the resources and instructions for deploying the Saleor e-commerce platform using Kubernetes. It includes a modular and scalable architecture with components like the Saleor API, Dashboard, PostgreSQL, Redis, and more.

## Table of Contents
- [Introduction](#introduction)
- [Architecture Overview](#architecture-overview)
- [Pre-requisites](#pre-requisites)
- [Setup Instructions](#setup-instructions)
  - [1. Clone the Repository](#1-clone-the-repository)
  - [2. Set Up Kubernetes Cluster](#2-set-up-kubernetes-cluster)
  - [3. Build and Push Docker Images](#3-build-and-push-docker-images)
  - [4. Deploy Kubernetes Resources](#4-deploy-kubernetes-resources)
  - [5. Expose Services](#5-expose-services)
  - [6. Verify Deployment](#6-verify-deployment)
- [Environment Variables](#environment-variables)
- [License](#license)

---

## Introduction
Saleor is a high-performance, headless e-commerce platform built with **Python**, **GraphQL**, **Django**, and **ReactJS**. This repository provides a guide to deploying Saleor on Kubernetes for development and production environments.

## Architecture Overview
The deployment architecture includes the following components:

- **Saleor API**: Manages backend logic and business operations.
- **Saleor Dashboard**: The front-end interface for admin management.
- **PostgreSQL**: Relational database for persistent data storage.
- **Redis**: In-memory key-value store for caching.
- **Mailpit**: Email server for sending transactional emails.
- **Worker**: Handles background tasks asynchronously.
  
Each component is deployed as a separate container and orchestrated using Kubernetes.

## Pre-requisites

- **Docker**: For containerizing applications.
- **Kubernetes Cluster**: You can use a managed Kubernetes service (e.g., Google Kubernetes Engine, AWS EKS) or a local cluster (e.g., Minikube).
- **kubectl**: Command-line tool to interact with Kubernetes clusters.
- **Helm (optional)**: Kubernetes package manager for deploying complex charts.
- **GCloud SDK**: For managing your GCP services (if deploying on Google Cloud).
- **Git**: Version control.

## Setup Instructions

### 1. Clone the Repository
Clone this repository to your local machine:
```bash
git clone https://github.com/your-username/saleor-platform.git
cd saleor-platform
```

### 2. Set Up Kubernetes Cluster
Create or access a Kubernetes cluster:
```bash
# Example for GKE:
gcloud container clusters create saleor-cluster --zone us-central1-a
```

Configure `kubectl` to interact with your cluster:
```bash
gcloud container clusters get-credentials saleor-cluster --zone us-central1-a
```

### 3. Build and Push Docker Images
Build and push Docker images to your registry:
```bash
# Build API image
docker build -t gcr.io/<your-project-id>/saleor-api:2.1 -f saleor/Dockerfile .
docker push gcr.io/<your-project-id>/saleor-api:2.1

# Build other images similarly (worker, dashboard, etc.)
```

### 4. Deploy Kubernetes Resources
Apply the Kubernetes manifests to deploy the platform:
```bash
kubectl apply -f k8s-configs/
```

This will deploy the necessary resources like Deployments, Services, and Persistent Volumes.

### 5. Expose Services
Expose services via LoadBalancer or Ingress, depending on your use case:
```bash
kubectl expose deployment saleor-api --type=LoadBalancer --name=api --port=8000
kubectl expose deployment saleor-dashboard --type=LoadBalancer --name=dashboard --port=9000
```

### 6. Verify Deployment
Check that all services are running properly:
```bash
kubectl get pods
kubectl get services
```

You should see the list of services (API, Dashboard, Redis, PostgreSQL, etc.) running with their external IP addresses if exposed.

## Environment Variables
Ensure that the following environment variables are configured in your Kubernetes manifests or secret management tool:

| Variable           | Description                               |
|--------------------|-------------------------------------------|
| `ALLOWED_HOSTS`     | Comma-separated list of allowed hosts     |
| `DATABASE_URL`      | PostgreSQL connection URL                |
| `REDIS_URL`         | Redis connection URL                     |
| `EMAIL_URL`         | URL for sending transactional emails     |
| `SECRET_KEY`        | Django secret key                        |
| `DEBUG`             | Debug mode (True/False)                  |
| `RSA_PRIVATE_KEY`   | Private key for JWT signing              |

## License
This repository is licensed under the BSD 3-Clause License. See the [LICENSE](LICENSE) file for more details.

---

## Conclusion

This guide walks through the deployment of Saleor on Kubernetes, leveraging Docker containers, external services like PostgreSQL and Redis, and Kubernetes networking. The architecture allows for scalability and easy management, making it ideal for both development and production environments.

For further questions or contributions, feel free to open an issue or a pull request.
