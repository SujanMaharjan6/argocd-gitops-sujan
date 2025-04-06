# Progressive Deployment with Argo Rollouts

## Prerequisites

- Minikube cluster is up and running (`minikube start`)
- Argo CD is installed and running in the `argocd` namespace
- Argo CD server is accessible via port-forwarding
- Logged in to Argo CD CLI (credentials and login were completed during the Cluster Management setup)
- `kubectl-argo-rollouts.exe` CLI tool downloaded

## Steps Performed

```bash
# Create namespace and install Rollouts
kubectl create namespace argo-rollouts
kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml

# Port-forward the dashboard
kubectl -n argo-rollouts port-forward deployment/argo-rollouts-dashboard 3100:3100
Note: Port-forwarding the Rollouts Dashboard (3100:3100) failed during the demo, so I used the CLI dashboard alternative.

# Change to Downloads folder (assuming the CLI is there)
cd ~
cd downloads

# Open the Argo Rollouts dashboard
.\kubectl-argo-rollouts.exe dashboard

# Create rollout app in Argo CD
argocd app create rollout-app --repo https://github.com/SujanMaharjan6/argocd-gitops-sujan.git --path rollout-app --dest-server https://kubernetes.default.svc --dest-namespace default --directory-recurse

# Sync and automate
argocd app sync rollout-app
argocd app set rollout-app --sync-policy automated --auto-prune --self-heal

# Rollout CLI actions
./kubectl-argo-rollouts.exe get rollout rollout-demo --watch
./kubectl-argo-rollouts.exe promote rollout-demo --namespace default --full
