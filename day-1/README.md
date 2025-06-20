# ArgoCD

ArgoCD is a gitops solutions to automate the sync of applications inside a k8s cluster using git repositories. It helps keeps the applications inside the cluster up to date and many more other things like canary deployments.

```bash
# extract the admin password from secrets
k -n argocd get secrets argocd-initial-admin-secret -o=jsonpath='{.data.password}' | base64 -d

# port foward the server
k -n argocd port-forward services/argocd-server 4443:443

# list applications
k -n argocd get applications
```

The database of the ArgoCD is already in the cluster, using ConfigMaps, etcd database, so its possible to backup the enteire cluster to another infraestructure and everything will run as expected.

**source**: where to pull the changes (repository, branch, tag, commit)
**destination**: where to put the changes (cluster URL, namespace)
