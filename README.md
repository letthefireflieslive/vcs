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
export DOCKER_USERNAME=[CHANGEME]
export DOCKER_PASSWORD=[CHANGEME]
export COUNTRY=[CHANGEME]
export ENV=[CHANGEME]
```
```
kubectl -n vcs-$COUNTRY-$ENV create secret docker-registry regcred --docker-username=$DOCKER_USERNAME --docker-password=$DOCKER_PASSWORD --docker-server="https://index.docker.io/v1/" --dry-run=client -o yaml | kubeseal -o yaml > overlays/$COUNTRY/$ENV/tooling/regcred.yml
git add . 
git commit -m "Add container registry and github PAT creds"
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
You can also manually hit the "referesh"/"sync" button in argo app

