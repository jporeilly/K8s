## <font color='red'> 8.2 Monitoring </font>
To scale an application and provide a reliable service, you need to understand how the application behaves when it is deployed. You can examine application performance in a Kubernetes cluster by examining the containers, pods, services, and the characteristics of the overall cluster. Kubernetes provides detailed information about an application's resource usage at each of these levels. This information allows you to evaluate your application's performance and where bottlenecks can be removed to improve overall performance.  

In Kubernetes, application monitoring does not depend on a single monitoring solution. On new clusters, you can use resource metrics or full metrics pipelines to collect monitoring statistics.  

In this lab we're going to Monitor:
* Kubernetes Metrics Server
* Prometheus
* Grafana
* Weave Scope
* Searchlight


---

#### <font color='red'> 8.2.1 Kubernetes Metrics Server </font>
to enable kubernetes metrics-server:
```
minikube addons enable metrics-server
```
rollout for kube-system:
```
kubectl -n kube-system rollout status deployment metrics-server
```
get kube-system PODs:
```
kubectl get pods --namespace kube-system
```
check node to seee if metrics are being collected:
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
check Pods perf:
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
minikube addons disable metrics-server
```

---

#### <font color='red'> Monitoring Tools </font>
check out the following monitoring tools:  
* Prometheus
* Grafana
* Prometheus Stack



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


#### <font color='red'> Prometheus Stack </font>
This stack is meant for cluster monitoring, so it is pre-configured to collect metrics from all Kubernetes components. In addition to that it delivers a default set of dashboards and alerting rules. Many of the useful dashboards and alerts come from the kubernetes-mixin project, similar to this project it provides composable jsonnet as a library for users to customize to their needs.  

kube-prometheus-stack is a collection of Kubernetes manifests including the follow
* Prometheus operator
* Prometheus
* Alertmanager
* Prometheus node-exporter
* Prometheus Adapter
* kube-state-metrics
* Grafana
* pre-configured to collect metrics from all Kubernetes component
* delivers a default set of dashboards and alerting rules

Prerequiste
* Kubernetes
* Helm

add prometheus helm repo:
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```
start minikube with the following command:
```
minikube start  --bootstrapper=kubeadm --extra-config=kubelet.authentication-token-webhook=true --extra-config=kubelet.authorization-mode=Webhook --extra-config=scheduler.address=0.0.0.0 --extra-config=controller-manager.address=0.0.0.0
```


create a namespace:
```
kubectl create namespace monitoring
```
install prometheus using helm:
```
helm install prometheus prometheus-community/kube-prometheus-stack --namespace monitoring
```
check were up and running:
```
kubectl get all --namespace monitoring
```
access Prometheus Dashboard: 
```
kubectl port-forward -n monitoring prometheus-prometheus-kube-prometheus-prometheus-0 9090
```

 > check in browser: http://localhost:9090

access Grafana Dashboard:
```
kubectl port-forward -n monitoring prometheus-grafana-xxxxxxx-xxxxx 3000
```
or just use the service:

 > http://[grafana-clusterIP]

default user: admin
password: prom-operator 

cleanup:
```
helm uninstall prometheus prometheus-community/kube-prometheus-stack --namespace monitoring
```
remove CRDs:
```
kubectl delete crd alertmanagerconfigs.monitoring.coreos.com
kubectl delete crd alertmanagers.monitoring.coreos.com
kubectl delete crd podmonitors.monitoring.coreos.com
kubectl delete crd probes.monitoring.coreos.com
kubectl delete crd prometheuses.monitoring.coreos.com
kubectl delete crd prometheusrules.monitoring.coreos.com
kubectl delete crd servicemonitors.monitoring.coreos.com
kubectl delete crd thanosrulers.monitoring.coreos.com
```





#### <font color='red'> Weave Scope </font>
install Weave Scope:
```
git clone https://github.com/weaveworks/scope
```
change to directory:
```
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
 > browse to http://127.0.0.1:4040

 
---  

#### <font color='red'> Searchlight </font>
Searchlight by AppsCode is a Kubernetes operator for Icinga. Searchlight periodically runs various checks on Kubernetes clusters and alerts you via email, SMS or chat if something goes wrong. Searchlight includes a default suite of checks written specifically for Kubernetes. Also, it can enhance Prometheus monitoring with external black-box monitoring and serves as a fallback in case internal systems completely fail.

Link: https://github.com/appscode/searchlight


#### <font color='red'> cAdvisor </font>
CAdvisor is installed by default on all cluster nodes to collect metrics for Kubernetes about running containers and nodes. CAdvisor Kubelet exposes these metrics through Kubelet APIs (with a default of one-minute resolution). The Metrics Server identifies all available nodes and calls Kubelet API to get containers and nodes resources usage before exposing the metrics through Kubernetes aggregation API.

Link: https://github.com/google/cadvisor
