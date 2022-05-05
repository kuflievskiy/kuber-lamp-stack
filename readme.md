minikube start

minikube addons enable ingress

kubectl apply -f dashboard-ingress.yaml && \
kubectl apply -f namespace-development.yaml && \
kubectl apply -f mysql-secret.yaml && \
kubectl apply -f mysql-configmap.yaml && \
kubectl apply -f httpd-deployment.yaml && \
kubectl apply -f httpd-service.yaml && \
kubectl apply -f mysql-statefulset.yaml && \
kubectl apply -f mysql-service.yaml && \
kubectl apply -f httpd-ingress.yaml

# kubectl get ingress -n kubernetes-dashboard --watch
# kubectl get ingress -n development --watch
# sudo vim /etc/hosts
# 192.168.64.2 k8s-dashboard.local
# 192.168.64.2 local.k8s.dev

minikube dashboard

echo "http://"$(kubectl get nodes --namespace development -o jsonpath="{.items[0].status.addresses[0].address}")":"$(kubectl get --namespace development -o jsonpath="{.spec.ports[0].nodePort}" services httpd-service)

kubectl exec -it [pod name] -- bash
kubectl get pod -n development | grep httpd | awk -F' ' '{print $1}'
kubectl exec -it --namespace=development httpd-deployment-b65b4ffb4-xq9tp -- bash
cd htdocs
apt-get update
apt-get install vim -y
vim index.html

minikube stop

kubectl delete -f dashboard-ingress.yaml && \
kubectl delete -f mysql-statefulset.yaml && \
kubectl delete -f mysql-service.yaml && \
kubectl delete -f httpd-ingress.yaml && \
kubectl delete -f mysql-configmap.yaml && \
kubectl delete -f mysql-secret.yaml && \
kubectl delete -f httpd-deployment.yaml && \
kubectl delete -f httpd-service.yaml && \
kubectl delete -f namespace-development.yaml
