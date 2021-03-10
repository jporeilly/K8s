## <font color='red'> 4.2 SSource Types </font>
Applications running in a Kubernetes cluster find and communicate with each other, and the outside world, through the Service abstraction.


I've added this section as a review


NAT
network address translation
Source NAT
replacing the source IP on a packet; in this page, that usually means replacing with the IP address of a node.
Destination NAT
replacing the destination IP on a packet; in this page, that usually means replacing with the IP address of a Pod
VIP
a virtual IP address, such as the one assigned to every Service in Kubernetes
kube-proxy
a network daemon that orchestrates Service VIP management on every node


The examples use a small nginx webserver that echoes back the source IP of requests it receives through an HTTP header. 



---


#### <font color='red'> 4.2.1 ClusterIP </font>
deploy nginx:
```
kubectl create deployment source-ip-app --image=k8s.gcr.io/echoserver:1.4
```
Packets sent to ClusterIP from within the cluster are never source NAT'd if you're running kube-proxy in iptables mode, (the default). You can query the kube-proxy mode by fetching http://localhost:10249/proxyMode on the node where kube-proxy is running.

get the nodes:
```
kubectl get nodes
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
kubectl create -f 02_nginx-service-clusterip.yaml --save-config
```
check service:
```
kubectl get svc nginx-service
```
Note: ClusterIP is default.

get details to check:
```
kubectl describe svc nginx-service
```
view endpoints:
```
kubectl get ep nginx-service
```
Note: You can try and access the Pod but no go as these are internal IPs. 

view nginx Pod IP:
```
kubectl get pods -l run=nginx -o wide
```


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


#### <font color='red'> 4.1. Accessing Services </font>
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


