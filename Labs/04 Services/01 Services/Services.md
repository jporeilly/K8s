## <font color='red'> 4.1 Services </font>
Now that you have a continuously running, replicated application you can expose it on a network.

Services are an abstract way to expose an application running on a set of Pods as a network service.
With Kubernetes you don't need to modify your application to use an unfamiliar service discovery mechanism. Kubernetes gives Pods their own IP addresses and a single DNS name for a set of Pods, and can load-balance across them.

K8s provides 4 types of Services:
* ClusterIP
* NodePort 
* Loadbalancer
* ExternalName

External:
* Ingress
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
deploy nginx:
```
kubectl create -f 01_nginx --save-config
```
check whats running:
```
kubectl get pods -l run=nginx -o wide
```
check POD IPs:
```
kubectl get pods -l run=nginx -o yaml | grep podIP
```
create a service:
```
kubectl expose deployment/nginx
```
or

YAML file:
```
kubectl create -f 02_nginx-service.yaml --save-config
```
check service:
```
kubectl get svc nginx
```
get details to check:
```
kubectl describe svc nginx
```
view endpoints:
```
kubectl get ep nginx
```

---


#### <font color='red'> 4.1.2 Access to Services </font>
For some parts of your applications you may want to expose a Service onto an external IP address.
Kubernetes supports two ways of doing this: 
* ClusterIP
* NodePorts
* LoadBalancers

**ClusterIP**
deploy clusterIP:
```
kubectl create deployment hello-world-clusterip  --image=gcr.io/google-samples/hello-app:1.0
```




#### <font color='red'> 4.1.2 Accessing Services </font>
Kubernetes supports 2 primary modes of finding a Service
* environment variables
* DNS

inspect environment:
```
kubectl exec nginx-xxxx -- printenv | grep SERVICE
```
Notice: no mention of service as replcas created before service.  

change deployment:
```
kubectl scale deployment nginx --replicas=0; kubectl scale deployment nginx --replicas=2;
```
inspect environment:
```
kubectl exec nginx-xxxxx -- printenv | grep SERVICE
```

inspect DNS:
```
kubectl get services kube-dns --namespace=kube-system
```

---

#### <font color='red'> 4.1.3 External Access to Services </font>
For some parts of your applications you may want to expose a Service onto an external IP address.
Kubernetes supports two ways of doing this: 
* ClusterIP
* NodePorts
* LoadBalancers

**ClusterIP**
deploy clusterIP:
```
kubectl create deployment hello-world-clusterip  --image=gcr.io/google-samples/hello-app:1.0
```
