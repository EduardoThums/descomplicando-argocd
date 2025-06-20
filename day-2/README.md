# Applications

**selfHeal**: ensures that the application in the cluster respects the repository state, if some manual change occurs.

# App of Apps

App of Apps is a architecture where we define a Application (sourcing from a repo or a helm chart) that contains other applications, this is useful when deploying fullstack resources when bootstraping a cluster like kube-prometheus, kyverno, ingress-nginx, cert-manager etc.

Its possible to pass parameters to the "Father" application to control which "Child" application inside of it will be deployed.

## Prune last

Prune is the act of remove old and obsolete resources from the argocd pool of applications, is a good pratice to ensure that only the need resources are being alive.

The problem with prune when using a app-of-apps application is when commit a new child-app the argocd (due to sync issues) might think that the child-app is not present in the father-app and delete it without ever creating it.

PruneLast tell argo to only run prune after all the sync is done. 

# Automatic Sync

Its possible to automatic sync applications by using webhooks and sync windows

## Webhook

1. Go to the project settings -> webhooks
2. Add a webhook that notifies every event
3. In localhost use ngrok to expose the HTTP url with the port-foward argocd service

# Sync External Helms

To sync external helm charts and pass values to it we use the `ref` field in the `sources`. Example:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: cert-manager
  namespace: argocd
spec:
  project: default
  destination:
    namespace: cert-manager
    server: https://kubernetes.default.svc
  sources:
    - repoURL: https://charts.jetstack.io
      chart: cert-manager
      targetRevision: v1.18.0
      helm:
        valueFiles:
          - $values/cert-manager/values.yaml

    - repoURL: https://github.com/EduardoThums/descomplicando-gitops-no-kubernetes-argocd.git
      targetRevision: main
      ref: values
```

We reference our own repository as the `ref` and use it in the `helm.valueFiles` section to specify the custom values to be passed to the helm chart.
