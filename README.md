argocd declarative configuration for docker-wildfly project

# Install all applications in all environments (using argocd root app - app of apps pattern)
kubectl apply -f root-argocd-app.yml
kubectl delete -f root-argocd-app.yml

# Install all applications in local environment only (using argocd appset)
kubectl apply -f appsets/my-local-appset.yml
kubectl delete -f appsets/my-local-appset.yml

kubectl apply -f appsets/my-dev-appset.yml
kubectl delete -f appsets/my-dev-appset.yml