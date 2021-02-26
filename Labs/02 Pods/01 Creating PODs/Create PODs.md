## <font color='red'> 2.1 Creating & Accessing PODs </font>

In this lab we're going to:
* Deploy an application
* Run through some common kubectl commands
* Shell into the Container
* Delete POD

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

#### <font color='red'> 2.1.2 Shell into POD </font>
to shell into the Container:
```
kubectl exec nginx -it -- sh
```
once inside:
```
ls -l
```
find the homepage:
```
cd usr/share/nginx/html
```
then:
```
ls -l
```
to exit:
```
exit
```
---

#### <font color='red'> 2.1.3 Deleting POD </font>
delete [pod-name]:
```
kubectl delete pod nginx
```
check PODs:
```
kubectl get pods
```

---