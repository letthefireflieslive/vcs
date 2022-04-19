# VCS

The whole build to K8s deployment configuration for VCS applications

## Prerequisites
- kubernetes
- argocd CLI
- argocd credentials
  `kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo`  

## Login to Argo
`argocd login`

## Create all VCS resources
`argocd app create vcs-root -f argo/root.yml`

# Maintenance

## Adding new Argo app or K8s resource? 
Add the entry at `./argo/apps`

## K8s Configuration Update?
Git commit and push it, ArgoCD will pick it up. 
You can also manually hit the "sync" button in argo app
