## <font color='red'> 8.2 Monitoring </font>

In this lab we're going to Monitor:
* Kubernetes Metrics Server
* Prometheus
* Weave Scope
* Searchlight
* Dynatrace

---

#### <font color='red'> 8.2.1 Kubernetes Metrics Server </font>
**The metrics server has already been installed**  

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
check out the following monitoring tools:  

* Prometheus
* Grafana

#### <font color='red'> Prometheus </font>
create a namespace:
```
kubectl create namespace monitoring
```
add prometheus helm repo:
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```
install prometheus using helm:
```
helm install prometheus prometheus-community/prometheus --namespace monitoring
```
to expose prometheus service:
```
kubectl expose service prometheus-server --type=NodePort --target-port=9090 --name=prometheus-server-np --namespace monitoring
```
access prometheus:
```
minikube service prometheus-server-np -n monitoring
```
or  

> in browser: http://192.168.49.2:31820/

* 
* 


#### <font color='red'> Grafana </font>
create a namespace:
```
kubectl create namespace monitoring
```
add grafana helm repo:
```
helm repo add grafana https://grafana.github.io/helm-charts
```
install grafana using helm:
```
helm install grafana stable/grafana --namespace monitoring
```
to expose grafana service:
```
kubectl expose service grafana --type=NodePort --target-port=3000 --name=grafana-np --namespace monitoring
```
grab grafana password for admin:
```
kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
access grafana:
```
minikube service grafana-np -n monitoring
```
or  

> in browser: http://192.168.49.2:32532/

* create a prometheus datasource - http://prometheus-server:80
* import grafana dashboard - 6417

---
