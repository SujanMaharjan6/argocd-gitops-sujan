# Application Management & Git Sync with Argo CD

Before starting:

- Minikube cluster is running (`minikube start`)
- Argo CD is installed and running in the `argocd` namespace
- Port-forwarding to Argo CD is active:
  ```bash
  kubectl port-forward svc/argocd-server -n argocd 8080:443
  Already logged into Argo CD via CLI (see Cluster Management section)
  
## Steps Performed

```bash
# Create the guestbook application
argocd app create guestbook-app --repo https://github.com/SujanMaharjan6/argocd-gitops-sujan.git --path guestbook-app --dest-server https://kubernetes.default.svc --dest-namespace default --directory-recurse --revision main

# Sync and automate
argocd app sync guestbook-app
argocd app set guestbook-app --sync-policy automated --auto-prune --self-heal

# Monitor Argo CD app state every 2 seconds
while ($true) {
  Get-Date
  argocd app get guestbook-app
  Start-Sleep -Seconds 2
}

# Monitor Argo CD app state every 5 seconds
while ($true) {
  Get-Date
  argocd app get guestbook-app
  Start-Sleep -Seconds 5
}

# Monitor guestbook deployment in Kubernetes
while ($true) {
  Get-Date
  kubectl get deploy guestbook-ui
  Start-Sleep -Seconds 10
}

# Scale down the deployment to simulate drift and trigger auto-heal
kubectl scale deploy guestbook-ui --replicas=1
