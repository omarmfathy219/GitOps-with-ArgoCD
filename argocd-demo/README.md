# ArgoCD GitOps Demo

## 🔐 ArgoCD Access

**Admin Credentials:**
- Username: `admin`
- Password: `z3XyFpBL8W2eOzGJ`

**Start ArgoCD UI:**
```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```
Access: https://localhost:8080

---

## ⚙️ Setup

**Before starting the demo:**

1. Update the GitHub repository URL in these files:
   - `applications/guestbook-app.yaml`
   - `applications/guestbook-staging.yaml`
   - `applications/guestbook-production.yaml`

2. Replace `YOUR-USERNAME/YOUR-REPO-NAME` with your actual GitHub repository

See `SETUP.md` for detailed instructions.

---

## 🚀 Demo Walkthrough

### 1. Deploy the Guestbook Application

```bash
kubectl apply -f argocd-demo/applications/guestbook-app.yaml
```

**In ArgoCD UI:**
- Application appears on dashboard
- Status: "OutOfSync" initially
- Click "SYNC" button (or wait for auto-sync)
- Watch the deployment in real-time

### 2. Access the Guestbook App

```bash
kubectl port-forward svc/guestbook-ui -n demo 8081:80
```
Open: http://localhost:8081

### 3. Demonstrate Self-Healing

**Manually change the cluster state:**
```bash
kubectl scale deployment guestbook-ui -n demo --replicas=5
```

**Watch ArgoCD:**
- Application shows "OutOfSync"
- Click on the app to see the diff
- ArgoCD auto-heals back to 3 replicas (as defined in Git)

**Check it reverted:**
```bash
kubectl get deployment guestbook-ui -n demo
```

### 4. Update via GitOps (The Right Way)

**Edit the manifest:**
```bash
# Edit: argocd-demo/guestbook/guestbook-ui-deployment.yaml
# Change: replicas: 3 → replicas: 5
```

**Commit the change:**
```bash
git add .
git commit -m "Scale guestbook to 5 replicas"
git push
```

**In ArgoCD UI:**
- Click "REFRESH" to detect changes faster
- Click "SYNC" or wait for auto-sync
- Watch deployment scale to 5 replicas

### 5. Rollback Demo

**In ArgoCD UI:**
1. Click on "History and Rollback" tab
2. Select a previous revision
3. Click "ROLLBACK"
4. Watch instant rollback

---

## 💡 Key Points to Demonstrate

1. **GitOps Workflow** - Git is the source of truth
2. **Automated Sync** - Changes deployed automatically
3. **Self-Healing** - Manual changes are reverted
4. **Visibility** - Beautiful UI with resource tree
5. **Rollback** - One-click rollback to any version
6. **Audit Trail** - All changes tracked with Git commits
7. **Declarative** - Everything defined as code

---

## 🧹 Cleanup

```bash
# Remove the demo app
kubectl delete application guestbook -n argocd
kubectl delete namespace demo

# Remove all environments
kubectl delete -f argocd-demo/applications/
kubectl delete namespace staging production
```

---

## 📁 Demo Structure

```
argocd-demo/
├── README.md                          # This file
├── DEMO-CHECKLIST.md                  # Step-by-step checklist
├── QUICK-REFERENCE.md                 # Quick command reference
├── argocd-ingress.yaml                # Ingress example
├── guestbook/                         # Application manifests
│   ├── namespace.yaml
│   ├── guestbook-ui-deployment.yaml
│   ├── guestbook-ui-service.yaml
│   ├── redis-deployment.yaml
│   └── redis-service.yaml
└── applications/                      # ArgoCD Application CRDs
    └── guestbook-app.yaml             # Demo application
```

