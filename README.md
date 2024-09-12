argocd declarative configuration for docker-wildfly project

# Install all applications in all environments (using argocd root app - app of apps pattern)
kubectl apply -f root-argocd-app.yml
kubectl delete -f root-argocd-app.yml

# Install all applications in local environment only (using argocd appset)
kubectl apply -f appsets/my-local-appset.yml
kubectl delete -f appsets/my-local-appset.yml

kubectl apply -f appsets/my-dev-appset.yml
kubectl delete -f appsets/my-dev-appset.yml

# Individual kustomise overlay
kubectl apply -k apps/wildfly/overlays/local
kubectl apply -k apps/wildfly/overlays/dev

kubectl delete -k apps/wildfly/overlays/local
kubectl delete -k apps/wildfly/overlays/dev

# For local machine!!!
## Install local certificate manager
kubectl apply -k ingress-controller/local-certificate-manager

