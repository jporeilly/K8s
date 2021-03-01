## <font color='red'> 7.3 Networking - Nodes </font>

In this lab we're going to examine the Kubernetes Network Model:
* Loadbalancer / NodePort

---

#### <font color='red'> 7.3.1 Loadbalancer / NodePort </font>
check whats running:
```
kubectl get all
```
deploy 2 PODs (replicaset):
```
kubectl create -f 01_loadbalancer-deployment.yaml --save-config
```
check PODs:
```
kubectl get pods -l run=load-balancer -o wide
```
deploy a virtual service:
```
kubectl expose deployment hello-world --type=NodePort --name=hello-world-service
```
lets take a deeper look at this service:
```
kubectl describe svc hello-world-service
```
Notice: Type NodePort  

check the endpoints:
```
kubectl get ep hello-world-service
```
Notice: 2 endpoints with mapped IPs to port 8080  

get public IP address of cluster:
```
kubectl cluster-info
```

> check request: http://[public-node-ip]:[node-port]

cleanup:
```
kubectl delete -f 01_loadbalancer-deployment.yaml
kubectl delete svc hello-world-service
```

---