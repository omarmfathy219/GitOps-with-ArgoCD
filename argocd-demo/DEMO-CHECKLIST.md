# ArgoCD Demo Checklist

## Before the Demo

- [ ] ArgoCD is running: `kubectl get pods -n argocd`
- [ ] Admin password saved: `z3XyFpBL8W2eOzGJ`
- [ ] Port-forward command ready in terminal
- [ ] Browser open to https://localhost:8080
- [ ] Clean ArgoCD dashboard (no existing apps)
- [ ] Git repository ready for commits

## During Demo

### Part 1: Deploy Application
- [ ] Run: `kubectl apply -f argocd-demo/applications/guestbook-app.yaml`
- [ ] Show application in ArgoCD UI
- [ ] Click SYNC and watch deployment
- [ ] Show resource tree view

### Part 2: Access Application
- [ ] Run: `kubectl port-forward svc/guestbook-ui -n demo 8081:80`
- [ ] Open http://localhost:8081 in browser
- [ ] Show working guestbook

### Part 3: Self-Healing
- [ ] Run: `kubectl scale deployment guestbook-ui -n demo --replicas=5`
- [ ] Show "OutOfSync" status in UI
- [ ] Watch ArgoCD auto-heal back to 3 replicas
- [ ] Explain: "Git is the source of truth"

### Part 4: GitOps Update
- [ ] Edit `guestbook-ui-deployment.yaml` (replicas: 3 → 5)
- [ ] Git commit and push
- [ ] Click REFRESH in ArgoCD UI
- [ ] Show sync and scaling

### Part 5: Rollback
- [ ] Show "History and Rollback" tab
- [ ] Select previous version
- [ ] Click ROLLBACK
- [ ] Show instant rollback

## After Demo

- [ ] Answer questions
- [ ] Share demo repository link
- [ ] Optional: Cleanup with `kubectl delete namespace demo`

