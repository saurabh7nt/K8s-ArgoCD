# Deployment Configurations

This directory contains deployment configurations for the Pharmacy Management application.

## Directory Structure

```
argocd/
├── argo/
│   └── argo-application.yml          # ArgoCD application definition
├── helm/
│   └── pharmacy-management-chart/    # Helm chart for deployment
│       ├── Chart.yaml                # Chart metadata
│       ├── values.yaml               # Default configuration
│       ├── values-prod.yaml          # Production overrides
│       └── templates/                # Kubernetes manifests
│           ├── deployment.yaml       # Backend services
│           ├── deployment.frontend.yaml
│           ├── service.yaml
│           ├── service.frontend.yaml
│           ├── statefulset.yaml      # PostgreSQL database
│           ├── postgres-service.yaml
│           └── ingress.yaml          # Routing rules
└── k8s/                              # Plain Kubernetes manifests
    ├── postgres/
    │   ├── service.yml
    │   └── statefulset.yml
    ├── user-service/
    │   ├── deployment.yml
    │   └── service.yml
    ├── medicine-service/
    │   ├── deployment.yml
    │   └── service.yml
    ├── order-service/
    │   ├── deployment.yml
    │   └── service.yml
    ├── frontend-service/
    │   ├── deployment.yml
    │   └── service.yml
    ├── ingress/
    │   └── ingress.yml
    └── kustomization.yaml            # Kustomize configuration
```

## Deployment Options

### Option 1: Using Helm (Recommended)

Helm manages all resources together with easy configuration.

## Quick Start

```bash
# Install the application
cd helm/pharmacy-management-chart
helm install pharmacy-app .

# Check status
kubectl get pods
kubectl get svc
kubectl get ingress

# Uninstall
helm uninstall pharmacy-app
```

**What's Deployed:**
- 3 Backend Microservices (User, Medicine, Order)
- Frontend React application
- PostgreSQL database
- Nginx ingress for routing

### Option 2: Using Plain Kubernetes Manifests

The `k8s/` folder contains individual YAML files for each service.

```bash
# Deploy all at once with Kustomize
kubectl apply -k k8s/

# Or deploy individually
kubectl apply -f k8s/postgres/
kubectl apply -f k8s/user-service/
kubectl apply -f k8s/medicine-service/
kubectl apply -f k8s/order-service/
kubectl apply -f k8s/frontend-service/
kubectl apply -f k8s/ingress/
```

### Option 3: Using ArgoCD

The `argo/` folder contains ArgoCD application definition for GitOps deployment.

```bash
# Apply ArgoCD application
kubectl apply -f argo/argo-application.yml
```

## Configuration

Edit `values.yaml` to change:
- Image tags
- Replica counts
- Database credentials
- Storage size
- Ingress settings

## Useful Commands

```bash
# Upgrade after changes
helm upgrade pharmacy-app .

# See what will be deployed
helm template pharmacy-app .

# Check for errors
helm lint .

# View logs
kubectl logs <pod-name>

# Port forward to test
kubectl port-forward svc/user-service-svc 8081:8081
```

## Access Application

After deployment, get the ingress IP:
```bash
kubectl get ingress
```

Then access:
- Frontend: `http://<INGRESS_IP>/`
- APIs: `http://<INGRESS_IP>/api/users`, `/api/medicines`, `/api/orders`

## Troubleshooting

```bash
# Check pod status
kubectl get pods
kubectl describe pod <pod-name>

# View logs
kubectl logs <pod-name>

# Check services
kubectl get svc
