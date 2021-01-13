## <font color='red'> 2.1 Creating & Accessing PODs </font>

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
