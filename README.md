#  ArgoCD GitOps CI/CD Pipeline

This repository demonstrates a fully automated **GitOps-based CI/CD pipeline** integrating:

> **GitHub Actions → Docker Hub → Argo CD → Kubernetes (Minikube)**

Whenever a developer pushes code to the main branch, the pipeline automatically:
- Builds a Docker image for the application  
- Pushes the image to **Docker Hub**  
- Updates the Kubernetes manifest (`k8s/deployment.yaml`) with the new image tag  
- Commits the change back to GitHub  
- Triggers **ArgoCD** to sync and deploy the updated version to the cluster  

---

##  Architecture Overview

![ArgoCD GitOps CI/CD Architecture](./images/argocd-pipeline.png)

**Flow Summary:**
1. Developer commits code to GitHub  
2. GitHub Actions builds & pushes Docker image  
3. Docker Hub stores versioned image  
4. Workflow updates deployment manifest  
5. Argo CD auto-syncs changes to Kubernetes  
6. Kubernetes deploys the new version  
7. End-user accesses the updated web app  

---

##  Technologies Used

| Category | Tool |
|-----------|------|
| **CI/CD** | GitHub Actions |
| **CD Platform** | ArgoCD |
| **Container Registry** | Docker Hub (`juvard/nginx-demo`) |
| **Containerization** | Docker |
| **Orchestration** | Kubernetes (Minikube) |
| **App** | Nginx static web app |
| **Scripting** | Bash, YQ for YAML updates |

---

## ⚙️ Pipeline Workflow

1. **Push to `main` branch**
   - Triggers the GitHub Actions workflow in `.github/workflows/ci-build-and-update.yaml`

2. **GitHub Actions**
   - Builds a Docker image  
   - Tags it using the commit SHA (e.g. `juvard/nginx-demo:479d4fc`)  
   - Pushes it to Docker Hub  
   - Uses `yq` to update `k8s/deployment.yaml` with the new image  
   - Commits & pushes the manifest change

3. **Argo CD**
   - Detects changes in the Git repo  
   - Automatically syncs the Kubernetes cluster with the new image tag

4. **Kubernetes**
   - Pulls the updated image  
   - Performs a rolling deployment to replace the previous version


---

##  How to Run Locally (Minikube)

```bash
# Start Minikube
minikube start

# Enable ingress (optional)
minikube addons enable ingress

# Deploy Kubernetes manifests
kubectl apply -f k8s/

# Get service URL
minikube service nginx-service --url
# Then open the printed URL in your browser to access the Nginx demo app.

# To confirm the deployment:
kubectl get pods
kubectl get svc nginx-service
kubectl describe deploy nginx-deploy | grep Image

Check the ArgoCD dashboard — the app should show:

Status: Healthy

Sync: Synced

Dockerhub: juvard/nginx-demo
# This project demonstrates GitOps best practices — declarative configuration, automatic reconciliation, and CI/CD decoupling.

Author

Jackson Ugochukwu




