curl -L https://git.io/getLatestIstio | sh -

export PATH="$PATH:/Users/casimir/Documents/AKS/istio-1.0.3/bin"

## HELM version V2.11.0
cd istio-1.0.3

helm install install/kubernetes/helm/istio --name istio --namespace istio-system

## Enable automatic Istio side car injection on default namespace

kubectl label namespace default istio-injection=enabled
