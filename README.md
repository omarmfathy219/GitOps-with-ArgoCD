# GitOps with ArgoCD - Demo

A comprehensive demo showcasing GitOps principles and ArgoCD continuous delivery for Kubernetes.

## 🎯 What's This?

This repository contains a complete ArgoCD demo with a sample guestbook application, designed to demonstrate:
- **GitOps workflow** - Git as the single source of truth
- **Automated deployments** - Changes in Git automatically deployed to Kubernetes
- **Self-healing** - Manual cluster changes automatically reverted
- **Rollback capability** - One-click rollback to any previous version
- **Multi-environment support** - Deploy to dev, staging, and production

Perfect for presentations, workshops, or learning ArgoCD!

## 📋 Prerequisites

- Kubernetes cluster (local or cloud)
- kubectl configured
- ArgoCD installed in `argocd` namespace

### Quick ArgoCD Installation

```bash
# Create namespace
kubectl create namespace argocd

# Install ArgoCD
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Get admin password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo

# Access ArgoCD UI
kubectl port-forward svc/argocd-server -n argocd 8080:443
# Then open: https://localhost:8080
```

## 🚀 Quick Start

### 1. Deploy the Demo Application

```bash
kubectl apply -f argocd-demo/applications/guestbook-app.yaml
```

### 2. Access ArgoCD UI

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Open: https://localhost:8080
- Login with username: `admin`
- Password: from the installation step above

### 3. Watch the Magic

- See the application appear in ArgoCD dashboard
- Click **SYNC** to deploy
- Watch real-time deployment in the beautiful UI

### 4. Access the Guestbook App

```bash
kubectl port-forward svc/guestbook-ui -n demo 8081:80
```

Open: http://localhost:8081

## 🎬 Complete Demo Walkthrough

For the full demo script with all steps, see:
- **[argocd-demo/README.md](argocd-demo/README.md)** - Complete walkthrough
- **[argocd-demo/DEMO-CHECKLIST.md](argocd-demo/DEMO-CHECKLIST.md)** - Step-by-step checklist
- **[argocd-demo/QUICK-REFERENCE.md](argocd-demo/QUICK-REFERENCE.md)** - Quick command reference

## 📁 Repository Structure

```
GitOps-with-ArgoCD/
├── README.md                          # This file
└── argocd-demo/
    ├── README.md                      # Complete demo walkthrough
    ├── DEMO-CHECKLIST.md              # Presentation checklist
    ├── QUICK-REFERENCE.md             # Quick commands
    ├── argocd-ingress.yaml            # Ingress example
    │
    ├── guestbook/                     # Application manifests
    │   ├── namespace.yaml
    │   ├── guestbook-ui-deployment.yaml
    │   ├── guestbook-ui-service.yaml
    │   ├── redis-deployment.yaml
    │   └── redis-service.yaml
    │
    └── applications/                  # ArgoCD Application CRDs
        ├── guestbook-app.yaml         # Main demo (auto-sync)
        ├── guestbook-staging.yaml     # Staging environment
        └── guestbook-production.yaml  # Production (manual sync)
```

## 💡 What You'll Learn

1. **GitOps Principles** - Declarative, version-controlled infrastructure
2. **Automated Sync** - How ArgoCD automatically deploys Git changes
3. **Self-Healing** - Watch ArgoCD revert manual cluster changes
4. **Drift Detection** - See immediate visibility when cluster != Git
5. **Rollback** - Instant rollback to any previous version
6. **Beautiful UI** - Visual resource tree and deployment status

## 🎯 Demo Scenarios

### Scenario 1: Self-Healing Demo
```bash
# Manually scale the deployment
kubectl scale deployment guestbook-ui -n demo --replicas=5

# Watch ArgoCD auto-heal back to 3 replicas
kubectl get deployment guestbook-ui -n demo -w
```

### Scenario 2: GitOps Update
```bash
# Edit the manifest (change replicas to 5)
vi argocd-demo/guestbook/guestbook-ui-deployment.yaml

# Commit and push
git add .
git commit -m "Scale to 5 replicas"
git push

# Watch ArgoCD sync the change automatically
```

### Scenario 3: Rollback
In ArgoCD UI:
1. Click on the application
2. Go to "History and Rollback"
3. Select a previous version
4. Click "ROLLBACK"

## 🧹 Cleanup

```bash
# Delete the demo application
kubectl delete application guestbook -n argocd
kubectl delete namespace demo

# Optional: Uninstall ArgoCD
kubectl delete -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl delete namespace argocd
```

## 📚 Resources

- **ArgoCD Documentation**: https://argo-cd.readthedocs.io/
- **GitOps Principles**: https://www.gitops.tech/
- **ArgoCD Best Practices**: https://argoproj.github.io/argo-cd/user-guide/best_practices/

## 🤝 Contributing

Feel free to fork this repository and adapt it for your own demos and presentations!

## 📄 License

This demo is provided as-is for educational purposes.

---

**Happy GitOps-ing! 🚀**
