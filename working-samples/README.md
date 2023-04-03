# Working Sample
This is a sample application that has some **base** k8s resources and then environment specific k8s resources available under **overlays** folder.

## Deploy to Staging Environment
Build and check the resources being created.

```
kustomize build overlays/staging
```

Depoy the resources

```
kubectl apply -k overlays/staging
```