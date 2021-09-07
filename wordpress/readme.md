minikube start

kubectl get nodes

helm search hub wordpress
helm search hub wordpress  --max-col-width=0

helm repo list
helm repo add bitnami https://charts.bitnami.com/bitnami

helm search repo wordpress --versions
helm show readme bitnami/wordpress --version 12.1.6

touch wordpress-values.yaml
vim wordpress-values.yaml

kubectl create namespace nswordpress
kubectl get namespace

helm install wordpress bitnami/wordpress --values=wordpress-values.yaml --namespace nswordpress --version 12.1.6


helm pull bitnami/wordpress --untar
helm uninstall wordpress bitnami/wordpress
####################################################################################################################


export NODE_PORT=$(kubectl get --namespace nswordpress -o jsonpath="{.spec.ports[0].nodePort}" services wordpress)
export NODE_IP=$(kubectl get nodes --namespace nswordpress -o jsonpath="{.items[0].status.addresses[0].address}")
echo "WordPress URL: http://$NODE_IP:$NODE_PORT/"