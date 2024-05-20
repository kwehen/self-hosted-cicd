### README.md

# Self-Hosted CI/CD Pipeline

This repository contains the code and configuration for a self-hosted CI/CD pipeline designed to build, push, and deploy container images to a Kubernetes cluster. The pipeline leverages Gitea for Git hosting and CI, Harbor for container registry, and ArgoCD for continuous deployment.

## Overview

The setup includes the following components:

- **Gitea**: Self-hosted Git service with CI capabilities.
- **Harbor**: Secure container registry with role-based access control and image vulnerability scanning.
- **ArgoCD**: GitOps continuous delivery tool for Kubernetes.
- **Kubernetes**: The orchestration platform for deploying and managing containerized applications.
- **Traefik**: Edge router to manage ingress for the Kubernetes cluster.

## Components and Configuration

### Gitea

Gitea is used for hosting Git repositories and managing CI/CD pipelines. A Docker-based runner is set up to execute CI jobs.

- `docker-compose.yml`: Configuration for running the Gitea runner in Docker.

### Harbor

Harbor is used to host container images securely within the homelab environment.

- `values.yaml`: Configuration file for deploying Harbor via Helm, specifying ingress rules, storage classes, and other settings.
- `certificate.yml`: Kubernetes manifest for creating a TLS certificate using cert-manager.
- `ingressroute.yml`: Traefik IngressRoute configuration for Harbor.

### CI/CD Pipeline

The CI/CD pipeline builds, tags, and pushes Docker images to Harbor and updates Kubernetes manifests via ArgoCD.

- `.github/workflows/ci.yml`: GitHub Actions workflow file for building, tagging, and pushing Docker images, and updating Kubernetes manifests.
- `Dockerfile`: Dockerfile for building the application container images.
- `manifest.yml`: Kubernetes deployment manifest for the application, updated by the CI pipeline.

## Usage

1. **Setup Harbor**: Deploy Harbor using the provided `values.yaml`, `certificate.yml`, and `ingressroute.yml` files.
2. **Configure Gitea**: Install Gitea and set up a runner using the `docker-compose.yml` file.
3. **Configure CI/CD Pipeline**: Add the CI pipeline configuration in `.github/workflows/ci.yml` to your repository.
4. **Deploy with ArgoCD**: Configure ArgoCD to watch the repository with the Kubernetes manifests and deploy changes automatically.

## Notes

- Make sure to update the `values.yaml`, `certificate.yml`, and `ingressroute.yml` files with your specific configuration settings.
- Ensure DNS records are properly configured for accessing the Harbor and Gitea services.

For detailed setup and usage instructions, refer to the relevant documentation for each tool.
