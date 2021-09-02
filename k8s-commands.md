### kubectl apply commands in order
minikube start && \
kubectl apply -f httpd.yaml && \
kubectl apply -f mysql-secret.yaml && \
kubectl apply -f mysql.yaml && \
kubectl apply -f mysql-configmap.yaml && \
kubectl apply -f httpd-ingress.yaml

#minikube service httpd-service




    kubectl apply -f mongo.yaml
    kubectl apply -f mongo-configmap.yaml 
    kubectl apply -f mongo-express.yaml

### kubectl get commands

    kubectl get pod
    kubectl get pod --watch
    kubectl get pod -o wide
    kubectl get service
    kubectl get secret
    kubectl get all | grep mongodb

### kubectl debugging commands

    kubectl describe pod mongodb-deployment-xxxxxx
    kubectl describe service mongodb-service
    kubectl logs mongo-express-xxxxxx

### give a URL to external service in minikube

    minikube service mongo-express-service


#### kompose convert

```
# https://gitlab.com/nanuchi/youtube-tutorial-series/-/blob/master/basic-kubectl-commands/cli-commands.md
```


# Deploy and Access the Kubernetes Dashboard
# https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
# https://github.com/kubernetes/dashboard/blob/master/docs/user/accessing-dashboard/README.m
````
minikube addons enable dashboard
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.3.1/aio/deploy/recommended.yaml

# Fix the dashboard role (this is not ideal because it gives your dashboard account cluster admin, and that is dangerous)
kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kubernetes-dashboard:kubernetes-dashboard

# Get the dashboard token
namespace=kubernetes-dashboard
token_name=kubernetes-dashboard
token=$(kubectl -n "${namespace}" get secret | grep "${token_name}" | cut -d " " -f1)
kubectl -n "${namespace}" describe secret $token

# Create a dashboard proxy
kubectl port-forward -n kubernetes-dashboard service/kubernetes-dashboard 443:443 --address 0.0.0.0
kubectl proxy
```

### HELM
# https://helm.sh/docs/intro/install/

```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh


curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
sudo apt-get install apt-transport-https --yes
echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```



helm search hub lamp

https://artifacthub.io/packages/helm/lamp/lamp
helm repo add lamp https://lead4good.github.io/lamp-helm-repository
helm install my-lamp lamp/lamp