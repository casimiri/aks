
# Install WordPress as a stateful application

## Install triller on server and client
helm init

## For RBAC enabled clusters

kubectl create serviceaccount --namespace kube-system tiller

kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller

kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'

## Using helm package for WordPress
https://github.com/helm/charts/tree/master/stable/wordpress

helm install --name kzwp stable/wordpress

## Connect to the deployed WordPress application
export SERVICE_IP=$(kubectl get svc --namespace default kzwp-wordpress --template "{{ range (index .status.loadBalancer.ingress 0) }}{{.}}{{ end }}")

echo "WordPress URL: http://$SERVICE_IP/"

echo "WordPress Admin URL: http://$SERVICE_IP/admin"

echo Username: user

echo Password: $(kubectl get secret --namespace default kzwp-wordpress -o jsonpath="{.data.wordpress-password}" | base64 --decode)
