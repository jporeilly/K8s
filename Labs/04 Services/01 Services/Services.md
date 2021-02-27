## <font color='red'> 4.1 Services </font>
Now that you have a continuously running, replicated application you can expose it on a network.

Services are an abstract way to expose an application running on a set of Pods as a network service.
With Kubernetes you don't need to modify your application to use an unfamiliar service discovery mechanism. Kubernetes gives Pods their own IP addresses and a single DNS name for a set of Pods, and can load-balance across them.

---

#### <font color='red'>IMPORTANT:</font> 
<strong>Please ensure you start with a clean environment. 
If you have previously run minikube, you will need to delete the existing instance.</strong>

to stop  minikube:
```
minikube stop
```
to delete  minikube:
```
minikube delete
```
start minikube:
```
minikube start
```
start tunnel:
```
minikube tunnel
```

--- 


#### <font color='red'> 4.1.1 Services </font>

check whats running:
```
kubectl get all
```
install my-nginx:
```
kubectl run my-nginx --image=nginx:alpine
```
check whats running:
```
kubectl get all
```
> Notice Deployment & Replicaset  

delete [pod-name]:
```
kubectl delete pod [pod-name]
```
check PODs:
```
kubectl get pods
```
> Notice new POD created

for port-forward:
```
kubectl port-forward [pod-name] 8000:80
```
check PODs:
```
kubectl get pods
```
check whats running:
```
kubectl get all
```
to delete a POD:
```
kubectl delete deployment my-nginx
```
check whats running:
```
kubectl get all
```
> Notice POD is Terminating  

set a Watch:
```
kubectl get pods --watch
```
---