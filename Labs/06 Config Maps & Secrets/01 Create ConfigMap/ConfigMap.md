## <font color='red'> 6.1 ConfigMaps </font>
Kubernetes Secrets and ConfigMaps separate the configuration of individual container instances from the container image, reducing overhead and adding flexibility.  
Kubernetes has two types of objects that can inject configuration data into a container when it starts up: 
* Secrets
* ConfigMaps. 
Secrets and ConfigMaps behave similarly in Kubernetes, both in how they are created and because they can be exposed inside a container as mounted files or volumes or environment variables.

In this lab were going to cover:



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