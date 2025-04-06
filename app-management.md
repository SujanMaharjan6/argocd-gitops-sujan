# Application Management & Git Sync with Argo CD

## Steps Performed

```bash
# Create the guestbook application
argocd app create guestbook-app \
  --repo https://github.com/SujanMaharjan6/argocd-gitops-sujan.git \
  --path guestbook-app \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default \
  --directory-recurse \
  --revision main

# Sync and automate
argocd app sync guestbook-app
argocd app set guestbook-app --sync-policy automated --auto-prune --self-heal

# Monitor app and deployment status
while ($true) {
  Get-Date
  argocd app get guestbook-app
  Start-Sleep -Seconds 2
}

while ($true) {
  Get-Date
  kubectl get deploy guestbook-ui
  Start-Sleep -Seconds 10
}

# Scale down deployment to trigger sync
kubectl scale deploy guestbook-ui --replicas=1
