# ArgoCD Demo - Quick Reference Card

## 🔐 Access Information

**ArgoCD Admin Credentials:**
- Username: `admin`
- Password: `z3XyFpBL8W2eOzGJ`

**Access URL:** https://localhost:8080

## 🚀 Essential Commands

### Start ArgoCD UI
```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

### Deploy Demo App
```bash
kubectl apply -f argocd-demo/applications/guestbook-app.yaml
```

### Access Guestbook UI
```bash
kubectl port-forward svc/guestbook-ui -n demo 8081:80
# Then open: http://localhost:8081
```

### Check Application Status
```bash
# Via kubectl
kubectl get applications -n argocd

# Get pods in demo namespace
kubectl get pods -n demo
```

### Demonstrate Self-Healing (Manual Scale)
```bash
# Scale up manually
kubectl scale deployment guestbook-ui -n demo --replicas=5

# Watch ArgoCD auto-heal it back to 3 replicas
kubectl get deployment guestbook-ui -n demo -w
```

### View ArgoCD Logs
```bash
kubectl logs -n argocd deployment/argocd-server -f
```

## 📊 Key UI Navigation

1. **Applications View** - Main dashboard showing all apps
2. **App Details** - Click app name to see resource tree
3. **Sync Status** - Green (Synced) / Yellow (OutOfSync)
4. **Health Status** - Healthy / Progressing / Degraded
5. **History & Rollback** - Click "History and Rollback" tab

## 🎯 Demo Checkpoints

- [ ] ArgoCD UI accessible
- [ ] Application deployed and synced
- [ ] Guestbook UI accessible via browser
- [ ] Self-healing demonstrated
- [ ] Git change deployed
- [ ] Rollback demonstrated

## 🔧 Troubleshooting

**Pods not starting?**
```bash
kubectl describe pod <pod-name> -n demo
kubectl logs <pod-name> -n demo
```

**Application stuck syncing?**
```bash
kubectl describe application guestbook -n argocd
```

**Can't access UI?**
```bash
# Check ArgoCD server is running
kubectl get pods -n argocd | grep server

# Check port-forward is active
lsof -i :8080
```

## 🧹 Quick Cleanup

```bash
# Remove demo app
kubectl delete application guestbook -n argocd
kubectl delete namespace demo

# Or just delete the namespace (cascade delete)
kubectl delete namespace demo
```

