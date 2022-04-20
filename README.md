# VCS

The whole build to K8s deployment configuration for VCS applications

## Prerequisites
- kubernetes
- kubectl
- kubeseal
- sealed secret controller
- argocd CLI
- argocd credentials
  `kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo`  

## Login to Argo
`argocd login`

## Create sealed secrets
```
kubectl -n vcs-eg-dev create secret docker-registry container-reg-creds --docker-username=[username] --docker-password=[password] --docker-email=[email] --docker-server=[server] --dry-run=client -o yaml | kubeseal -o yaml > overlays/eg/dev/tooling/container-registry-creds.yml
git add . 
git commit -m "Add container registry creds"
git push
```

## Create all VCS resources
```
argocd proj create vcs -f argo/project.yml
argocd app create vcs-root -f argo/root.yml
```

# Maintenance

## Adding new Argo app or K8s resource? 
Add the entry at `./argo/apps`

## K8s Configuration Update?
Git commit and push it, ArgoCD will pick it up. 
You can also manually hit the "sync" button in argo app

