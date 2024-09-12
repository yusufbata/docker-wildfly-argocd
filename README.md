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
## Install certificate manager
?????

## Add DNS entries for ingress host names
???

## Test access
Check secret:
kubectl -n local get secret mywildfy-cert -o jsonpath='{.data.ca\.crt}' | base64 -d

Test access using Curl:
curl --cacert <(kubectl -n local get secret mywildfy-cert -o jsonpath='{.data.ca\.crt}' | base64 -d) https://myservicea.test

## Trust local test cert on local machine
For MAC (https://docs.blackberry.com/en/unified-endpoint-security/cylanceonprem/cylance-on-prem-administration-guide/Agent/Add-root-CA-certificate-to-every-endpoint/Add_a_Root_CA_Certificate_to_macOS):

Store cert in file (my interim step):
kubectl get secret test-ca-secret -n local -o jsonpath='{.data.ca\.crt}' | base64 -d >> testRootCA.crt

Now add to mac trusted certificates:
sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain testRootCA.crt

Delete temporary file:
rm testRootCA.crt

Test (without including local --cacert parameter from k8):
curl -Li http://myservicea.test
curl -Li http://myserviceb.test

# TODO
- Install cert manager as app
- Install argocd
- Install ingress-nginx