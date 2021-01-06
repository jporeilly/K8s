## <font color='red'> 1.1 K8s Cluster </font>

> Adapted from the [Kubernetes Tutorials](https://kubernetes.io/docs/tutorials/)  

ensure minikube is up and running:
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

in a new terminal access dashboard:
```
minikube dashboard
```

---

### <font color='red'> 1.1.1 Create  a Deployment </font>
check whats running on Kubernetes:
```
kubectl get all
```
deploy a Pod / Container:
```
kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4
```
