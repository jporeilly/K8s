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
create service - YAML:
```
kubectl create -f 02_nginx-service-clusterip.yaml --save-config
```
or

create a service (use the YAML manifest):
```
kubectl expose deployment/nginx-service
```
check service:
```
kubectl get svc nginx-service
```
Note: ClusterIP is default.

describe service:
```
kubectl describe svc nginx-service
```
view endpoints:
```
kubectl get ep nginx-service
```
Note: You can try and access the Pod in a browser but no go as these are internal IPs. 

view nginx Pod IP:
```
kubectl get pods -l run=nginx -o wide
```
access the Service with a browser:

> http://[service-ip]

proxy the request:
```
kubectl proxy --port 8080
```
to access the Pods:

 > http://localhost:8080/api/v1/namespaces/default/pods

to access the Service:

 > http://localhost:8080/api/v1/namespaces/default/service/nginx-service/

Note: this just connects to the service and is useful for debugging

also try mapping Service IP to localhost in /etc/hosts file..


clean up:
```
kubectl delete -f 01_nginx.yaml
kubectl delete -f 02_nginx-service-clusterip.yaml
```

---

#### <font color='red'> 4.1.2 Services - NodePort </font>
A NodePort is an open port on every node of your cluster. Kubernetes transparently routes incoming traffic on the NodePort to your service, even if your application is running on a different node.


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
kubectl create -f 03_nginx-service-nodeport.yaml
```

check service:
```
kubectl get svc nginx-service-nodeport
```
describe service:
```
kubectl describe svc nginx-service-nodeport
```
view endpoints:
```
kubectl get ep nginx-service-nodeport -o yaml
```
to access the Service:

 > http://[nodeport-ip]:31000


clean up:
```
kubectl delete -f 01_nginx.yaml
kubectl delete -f 03_nginx-service-nodeport.yaml
```

---

#### <font color='red'> 4.1.2 Services - LoadBalancers </font>
Loadbalancers c

check whats running:
```
kubectl get all
```
deploy nginx:
```
kubectl apply -f 04_nginx-loadbalancer.yaml
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
kubectl apply -f 05_nginx-service-loadbalancer.yaml


check service:
```
kubectl get svc nginx-service-loadbalancer
```
describe service:
```
kubectl describe svc nginx-service-loadbalancer
```
view endpoints:
```
kubectl get ep nginx-service-loadbalancer -o yaml
```
to access the Service:

 > http://[external-ip]


clean up:
```
kubectl delete -f 01_nginx.yaml
kubectl delete -f 03_nginx-service-nodeport.yaml
```

---

#### <font color='red'> 4.1.3 External Access to Services </font>
For some parts of your applications you may want to expose a Service onto an external IP address.
Kubernetes supports two ways of doing this: 
* ClusterIP
* NodePorts
* LoadBalancers


