
cd ../AKS/istio-1.0.3/

## Deploy the BookInfo application

kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml

### Confirm all services and pods

kubectl get services

kubectl get pods

## ingress IP and port

kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml

### Confirm the gateway

kubectl get gateway

## INGRESS_HOST and INGRESS_PORT

### Provider supports LoadBalancer?

kubectl get svc istio-ingressgateway -n istio-system

### Set Ingress IP and Port

export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')

export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')

export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].port}')

### Set Gateway URL

export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT

### Confirm the App is running

curl -o /dev/null -s -w "%{http_code}\n" http://${GATEWAY_URL}/productpage

Browse: http://${GATEWAY_URL}/productpage

### Apply default destination rule

kubectl apply -f samples/bookinfo/networking/destination-rule-all.yaml

kubectl get destinationrules



