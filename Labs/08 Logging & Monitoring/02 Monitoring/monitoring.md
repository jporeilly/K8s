## <font color='red'> 8.2 Monitoring </font>
o scale an application and provide a reliable service, you need to understand how the application behaves when it is deployed. You can examine application performance in a Kubernetes cluster by examining the containers, pods, services, and the characteristics of the overall cluster. Kubernetes provides detailed information about an application's resource usage at each of these levels. This information allows you to evaluate your application's performance and where bottlenecks can be removed to improve overall performance.  

In Kubernetes, application monitoring does not depend on a single monitoring solution. On new clusters, you can use resource metrics or full metrics pipelines to collect monitoring statistics.  

In this lab we're going to Monitor:
* Kubernetes Metrics Server
* Prometheus
* Grafana
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

---

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

#### <font color='red'> Weave Scope </font>
install Weave Scope:
```
git clone https://github.com/weaveworks/scope
cd scope
```
to deploy to cluster:
```
kubectl apply -f examples/k8s -n monitoring
```
port forward requests to 4040:
```
kubectl port-forward svc/weave-scope-app -n weave 4040:80
```

---  

#### <font color='red'> Searchlight </font>



helm repo add appscode https://charts.appscode.com/stable/



helm install appscode/searchlight --name searchlight-operator --namespace kube-system


helm delete searchlight-operator