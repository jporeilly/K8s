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
kubectl create -f 01_nginx-deployment.yaml --save-config
```
so lets take a look at the Containers:
```
docker ps
```




check PODs:
```
kubectl get pods -l run=nginx -o wide
```
write out (YAML) POD IPs:
```
kubectl get pods -l run=nginx -o yaml | grep podIP
```
Note: the PODs are not using port 80 on the Node and there isn't any special NAT rules to route the traffic to the POD.  

deploy a service:
```
kubectl create -f 02_nginx-svc.yaml --save-config
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
kubectl get ep nginx-svc
```
Notice: 2 endpoints with mapped IPs to port 80  

> check request: http://[Cluster IP]

cleanup:
```
kubectl delete -f 01_nginx-deployment.yaml
kubectl delete -f 02_nginx-svc.yaml
```

**Production Environment**  

break this down to the network level:
```
sudo netstat -pant | grep LISTEN
```  

take a look at the IP tables to see the rules:
```
sudo iptables -t nat -L KUBE-SERVICES
```
then look at the rules:
```
sudo iptables -t nat -L KUBE-xxxx
```
Notice: chain requests

---