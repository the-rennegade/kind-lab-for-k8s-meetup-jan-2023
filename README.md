# Lab Setup

## Cluster Setup and PreReqs 
1. `kind create cluster --config kind-cluster-config.yml`
2. `helm repo add appscode https://charts.appscode.com/stable/`
3. `helm repo add jetstack https://charts.jetstack.io`
4. `helm repo update`
5. `helm install kubed appscode/kubed --version v0.13.2 --namespace kube-system`

# Demos

## Helm
1. `helm install hello-world . -f values.yaml --create-namespace --namespace hello-world`
2. `kubectl get svc -n hello-world`
3. `kubectl port-forward service/hello-world -n hello-world 8000:80`
4. `helm status hello-world -n hello-world`
5. `kubectl get pods -n hello-world`
6. Change replica count in values file and chart version.
7. `helm upgrade hello-world . -f values.yaml --namespace hello-world`
8. `helm history hello-world -n hello-world`
9. `kubectl get pods -n hello-world`


## Helm Secrets
1. Review .sops.yaml file.
2. Review unencrypted secrets.yaml file.
3. `gpg --list-secret-keys`
4. Uncomment the env var on the deployment template.
5. `helm secrets enc secrets.yaml`
6. `helm secrets upgrade hello-world . -f values.yaml -f secrets.yaml --namespace hello-world`
7. `kubectl get pods -n hello-world -o yaml hello-world-65f8876984-htwpz`

## Kubent
1. `kubent`

## Config-Syncer
1. Review tls secret.
2. Change value to enable ingress and create secret.
3. Modify the template
4. `helm secrets upgrade hello-world . -f values.yaml -f secrets.yaml --namespace hello-world`
5. `kubectl get secrets -n hello-world`
6. Uncomment kubed annotation on tls secret template.
7. `kubectl create ns hello-world2`
8. `helm secrets upgrade hello-world . -f values.yaml -f secrets.yaml --namespace hello-world`
9. `kubectl get secrets -A`
10. Change value to disable ingress and delete secret.
11. ``helm secrets upgrade hello-world . -f values.yaml -f secrets.yaml --namespace hello-world`

## Helmfile
1. Review helmfile.yaml.
2. `helmfile apply --file helmfile.yaml`
3. `helm list -A`
4. Uncomment the grafana sections of the Helmfile.
5. `helmfile apply --file helmfile.yaml`
6. `helm list -A`
7. Change rbac.create in Helmfile to "true".
8. `helmfile diff --file helmfile.yaml`
9. `helmfile apply --file helmfile.yaml`
10. Go over the caveat with removing a release.

## Cert-Manager
1. `helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --version v1.10.1 --set installCRDs=true`
2. Review the self-signed-issuer yaml file.
3. `kubectl apply -f ./cert-manager/self-signed-issuer.yaml`
4. `kubectl get clusterissuers`
5. Review the ca-and-certs.yaml file.
6. `kubectl apply -f ./cert-manager/ca-and-certs.yaml`
7. `kubectl get certificates -n sandbox`
8. `kubectl get secrets -n sandbox`
9. `kubectl get secrets -n sandbox -o yaml root-secret`
10. `kubectl get issuers -n sandbox`