## <font color='red'> 7.2 Networking - PODs & Services </font>

In this lab we're going to examine the Kubernetes Network Model:
* PODs & Services
* Cluster IP
* Node IP
* Loadbalancing

---

#### <font color='red'> 7.2.1 PODs & Services </font>
check whats running:
```
kubectl get all
```
deploy 2 PODs (replicaset):
```
kubectl create -f 01_nginx-deployment.yaml
```
check PODs:
```
kubectl get pods -l run=nginx -o wide
```
get the POD IPs:
```
kubectl get pods -l run=my-nginx -o yaml | grep podIP
```
Note: the PODs are not using port 80 on the Node and there isn't any special NAT rules to route the traffic to the POD.  

deploy a service:
```
kubectl create -f 02_nginx-svc.yaml
```
or you could use:
```
kubectl expose deployment/nginx-svc
```
check IP and port of nginx service:
```
kubectl get svc nginx-svc
```
Notice: the service is assigned a unique ClusterIP exposed on port 80.

lets take a deeper look at the service:
```
kubectl describe svc nginx-svc
```
Notice: 1 Cluster IP thats servicing 2 POD endpoints.  

check the endpoints:
```
kubectl get ep nginx
```
Note: MAC address  

network isolation c2:
```
kubectl exec -it podtest -c c2 -- ip link
```
Note: both containers have the device: eth0 and MAC addresses.  

connect to c1:
```
kubectl exec -it podtest -c c1 -- sh
```
start newtwork listener:
```
nc -lk -p 5000 127.0.0.1 -e 'date'
```
in a new terminal:
```
kubectl exec -it podtest -c c2 -- sh
```
telnet to c1:
```
telnet localhost:5000
```
Note: cant see nc process but can see date.  So containers have process isolation but not network.


---


#### <font color='red'> 2.2.2 Deleting PODs </font>
to delete the POD:
```
kubectl delete pod nginx
```
can also use:
```
kubectl delete -f 01_nginx-pod.yaml
```  

---

#### <font color='red'> 2.2.3 Deployment YAML & PODs </font>
check whats running:
```
kubectl get all
```
create nginx deployment:
```
kubectl create -f 02_nginx-deployment.yaml
```
then issue the command again:
```
kubectl create -f 02_nginx-deployment.yaml
```
Notice: an error will occur as the POD already exisits.

use apply command:
```
kubectl apply -f 02_nginx-deployment.yaml
```
Notice: Declarative as state is applied to resource.  If doesnt exist then created.

check the YAML:
```
kubectl get pod nginx-xxxxxx -o yaml
```
---

#### <font color='red'> 2.2.4 Delete Deployment YAML & PODs </font>
check whats running:
```
kubectl get all
```

to delete the POD:
```
kubectl delete pod nginx-xxxxx
```
Notice: the POD is deleted but another is created.
Note: the reason why you can delete the POD is that kind: POD is Deployment.

to delete:
```
kubectl delete -f 01_nginx.pod.yaml
``` 
or 
```
kubectl delete deployment.apps/nginx-deployment 
```

> YAML spec: https://yaml.org/spec/1.2/spec.html

---