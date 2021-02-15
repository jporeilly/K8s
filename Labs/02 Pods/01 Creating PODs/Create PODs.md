## <font color='red'> 2.1 Creating & Accessing PODs </font>

In this lab we're going to:
* Deploy an application
* Run through some common kubectl commands
* Delete POD
---

#### <font color='red'> 2.1.1 Creating POD </font>
check whats running:
```
kubectl get all
```
install nginx:
```
kubectl run nginx --image=nginx:alpine
```
check whats running:
```
kubectl get all
```
> Notice: Deployment & Replicaset  

check PODs:
```
kubectl get pods
```
for port-forward:
```
kubectl port-forward nginx 9000:80
```

> check in browser: http://localhost:9000

lets stop the port forwarding:
```
Ctrl+C
```
check PODs:
```
kubectl get pods
```

---

#### <font color='red'> 2.1.2 Deleting POD </font>
delete [pod-name]:
```
kubectl delete pod nginx
```
check PODs:
```
kubectl get pods
```

---