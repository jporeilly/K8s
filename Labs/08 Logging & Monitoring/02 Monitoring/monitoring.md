## <font color='red'> 8.2 Monitoring </font>

In this lab we're going to Monitor:
* Kubernetes Metrics Server
* Prometheus
* Weave Scope
* Searchlight
* Dynatrace

---

#### <font color='red'> 8.2.1 Kubernetes Metrics Server </font>
**The metrics server has already been installed
deploy latest release:
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
get kube-system PODs:
```
kubectl get pods --namespace kube-system
```
check node to seee if logs are being collected:
```
kubectl top nodes
```
Note: you may have to give it a few minutes..  

system PODs, CPU and memory::
```
kubectl top pods --all-namespaces
```
deploy a POD that will burn CPU:
```
kubectl apply -f 01_cpuburner.yaml
```  
deploy nginx and scale:
```  
kubectl create deployment nginx --image=nginx
kubectl scale  deployment nginx --replicas=3
``` 
check PODs:
```
kubectl get pods -o wide
``` 
check node:
```
kubectl top nodes
```
check PODs perf:
```
kubectl top pods
```
can specify POD:
```
kubectl top pods -l app=cpuburner
```
can sort perf:
```
kubectl top pods --sort-by=cpu
kubectl top pods --sort-by=memory
```
check containers:
```
kubectl top pods --containers
```
clean up:
```
kubectl delete deployment cpuburner
kubectl delete deployment nginx
```

---

#### <font color='red'> Monitoring Tools </font>
check out the following logging tools:
* kubebox  - 
* kubetail - aggregate logging from multiple PODs
* kubewatch - publishes events to Slack