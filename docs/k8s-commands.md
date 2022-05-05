#### https://minikube.sigs.k8s.io/docs/start/
#### macOS, x86-64, Stable, Binary download
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube

minikube start
minikube dashboard
minikube status
minikube stop

#### 

kubectl apply -f [file name].yaml
kubectl delete -f [file name].yaml

kubectl exec -it [pod name] -- bash

#### kubectl get/edit commands

kubectl get pod
kubectl get pod --watch
kubectl get pod -o wide
kubectl get service
kubectl get secret
kubectl get all | grep mongodb

kubectl get deployment [deployment name] -o yaml > [deployment name]-result.yaml

kubectl create deployment [deployment name] --image=<image-name>
kubectl edit deployment <name>
kubectl delete deployment <name>

#### kubectl debugging commands

kubectl describe pod [pod name]
kubectl describe pod mongodb-deployment-xxxxxx

kubectl describe service mongodb-service
kubectl logs [pod name]

#### give a URL to external service in minikube

minikube service [service name]


#### Deploy and Access the Kubernetes Dashboard
#### https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
#### https://github.com/kubernetes/dashboard/blob/master/docs/user/accessing-dashboard/README.m
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

