## <font color='red'> 4.1 Services </font>
Now that you have a continuously running, replicated application you can expose it on a network.

Services are an abstract way to expose an application running on a set of Pods as a network service.
With Kubernetes you don't need to modify your application to use an unfamiliar service discovery mechanism. Kubernetes gives Pods their own IP addresses and a single DNS name for a set of Pods, and can load-balance across them.

K8s provides 4 types of Services:
* ClusterIP - exposes IPs internally in the Cluster
* NodePort - exposes IPs externally
* Loadbalancer
* ExternalName

External:
* Ingress
* Egress


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

#### <font color='red'> 4.1.1 Services - ClusterIP </font>

check whats running:
```
kubectl get all
```
deploy nginx:
```
kubectl create -f 01_nginx.yaml --save-config
```
check whats running:
```
kubectl get pods -l run=nginx -o wide
```
check Pod IPs:
```
kubectl get pods -l run=nginx -o yaml | grep podIP
```
create a service:
```
kubectl expose deployment/nginx-service
```
or

YAML file:
```
kubectl create -f 02_nginx-service.yaml --save-config
```
check service:
```
kubectl get svc nginx-service
```
get details to check:
```
kubectl describe svc nginx-service
```
view endpoints:
```
kubectl get ep nginx-service
```
Note: You can try and access the Pod but no go as this is internal IPs. 

view nginx Pod IP:
```
kubectl get pods -l run=nginx -o wide
```
Note: ClusterIP is default.

clean up:
```
kubectl delete -f 02_nginx-service.yaml
```
Note: just delete nginx sevice.

---

#### <font color='red'> 4.1.2 Services - NodeIP </font>


lets scale up our nginx:
```
kubectl scale deployments/nginx --replicas=3
```
check whats running:
```
kubectl get pods -l run=nginx -o wide
```
check Pod IPs:
```
kubectl get pods -l run=nginx -o yaml | grep podIP
```
deploy clusterip service:
```
kubectl apply -f 03_nginx-service-clusterip.yaml
```
check service:
```
kubectl get svc nginx-service-clusterip
```
get details to check:
```
kubectl describe svc nginx-service-clusterip
```
view endpoints:
```
kubectl get ep nginx-service-clusterip -o yaml
```

clean up:



---


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


