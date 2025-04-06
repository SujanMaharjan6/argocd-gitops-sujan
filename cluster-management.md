# Cluster Management via Argo CD

## 🧪 Steps Performed

```bash
# Start minikube
minikube start

# Create Argo CD namespace and install Argo CD
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Check Argo CD pods
kubectl get pods -n argocd

# Port-forward Argo CD server
kubectl port-forward svc/argocd-server -n argocd 8080:443

# Install Argo CD CLI
choco install argocd-cli

# Get initial admin password
argocd admin initial-password -n argocd

# Login to Argo CD
argocd login localhost:8080 --username admin --password odMkuc7Dly0dLdbO

# Create a namespace management app from Git
argocd app create namespace-manager --repo https://github.com/SujanMaharjan6/argocd-gitops-sujan.git --path cluster-configs --dest-server https://kubernetes.default.svc --dest-namespace default --directory-recurse --revision main

# Sync and automate the app
argocd app sync namespace-manager
argocd app set namespace-manager --sync-policy automated --auto-prune --self-heal

# Monitor created namespaces and roles
kubectl get ns
kubectl get ns --watch
kubectl get rolebinding -n dev --watch
