## <font color='red'> 1.1 K8s Cluster </font>

minikube is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes.

fire up minikube:
```
minikube start
```

check minikube status:
```
minikube status
```

view addons:
```
minikube addons list
```

For additional insight into your cluster state, minikube bundles the Kubernetes Dashboard:
in a new terminal access dashboard:
```
minikube dashboard
```
> For further information: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
--- 

additional minikube commands.  

upgrade cluster:
```
minikube start --kubernetes-version=latest
```
stop your local cluster:
```
minikube stop
```
delete your local cluster:
```
minikube delete
```
---
