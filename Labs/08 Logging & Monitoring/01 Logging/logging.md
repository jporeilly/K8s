## <font color='red'> 8.1 Logging </font>
Application logs can help you understand what is happening inside your application. The logs are particularly useful for debugging problems and monitoring cluster activity. Most modern applications have some kind of logging mechanism. Likewise, container engines are designed to support logging. The easiest and most adopted logging method for containerized applications is writing to standard output and standard error streams.  

In this lab we're going to examine Logging:
* Containers & PODs
* Nodes
* Control Plane
* Events

---

#### <font color='red'> 8.1.1 Logging Containers / PODs </font>
**single container**  

deploy a busybox Pod:
```
kubectl create -f 01_single-container.yaml
```
set the variable for the Pod:
```
PODNAME=$(kubectl get pods -l app=busybox -o jsonpath='{ .items[0].metadata.name }')
```
check variable:
```
echo $PODNAME
```
access busybox Pod logs:
```
kubectl logs $PODNAME
```
can see the logs in realtime:
```
kubectl logs busybox -f
```

clean up:
```
kubectl delete deployment busybox
```  

---

**multi-container**  

deploy busybox multi-container:
```  
kubectl create -f 01_multi-container.yaml --save-config
``` 
set the variable for the POD:
```
PODNAME=$(kubectl get pods -l app=logging-demo -o jsonpath='{ .items[0].metadata.name }')
``` 
check variable:
```
echo $PODNAME
```
access Busybox POD logs:
```
kubectl logs $PODNAME
```
Note: Error as multiple containers  

can specify container:
```
kubectl logs $PODNAME -c c1
kubectl logs $PODNAME -c c2
```
all container logs:
```
kubectl logs $PODNAME --all-containers
```
follow a log:
```
kubectl logs $PODNAME --all-containers --follow
```
to stop:
```
ctrl+c
```
across all PODs:
```
kubectl get pods --selector app=logging-demo
kubectl logs --selector app=logging-demo --all-containers 
kubectl logs --selector app=logging-demo --all-containers  > allpods.txt
```
can also tail the logs:
```
kubectl logs --selector app=logging-demo --all-containers --tail 5
```
Note: 5 records from c1 & c2

clean up:
```
kubectl delete -f 01_multi-container.yaml
```

---

#### <font color='red'> 8.1.2 Logging Nodes </font>
ssh into minikube:
```
minikube ssh
```
kubelet status:
```
systemctl status kubelet.service
```
exit minikube:
```
exit
```
a journal daemon is implemented for log collection:
```
journalctl -u kubelet.service
```
use grep to search logs:
```
journalctl -u kubelet.service | grep -i ERROR
```
time bound the search:
```
journalctl -u kubelet.service --since today --no-pager
```
Note: --no-pager for line wrapping

---

#### <font color='red'> 8.1.3 Control Plane </font>
get a list of control plane PODs:
```
kubectl get pods --namespace kube-system --selector tier=control-plane
```
retrieve logs:
```
kubectl logs --namespace kube-system kube-apiserver-minikube
```
set docker environment:
```
eval $(minikube docker-env)
```
if control plane is down:
```
sudo docker ps
```
log for api-server:
```
docker ps  | grep k8s_kube-apiserver
CONTAINER_ID=$(docker ps | grep k8s_kube-apiserver | awk '{ print $1 }')
echo $CONTAINER_ID
docker logs $CONTAINER_ID
```


ssh into minikube:
```
minikube ssh
```
and if Docker is down:
```
ls /var/log/containers
tail /var/log/containers/kube-apiserver-[master-container]*
```
Note: Logs menu options in VSC. 

---

#### <font color='red'> 8.1.4 Events </font>
events for all objects in default namespace:
```
kubectl get events 
```
sort by timestamp:
```
kubectl get events --sort-by='.metadata.creationTimestamp'
```
create a error:
```
kubectl create deployment nginx --image ngins
```
filter for events:
```
kubectl get events --field-selector type=Warning
kubectl get events --field-selector type=Warning,reason=Failed
```
monitor events with Watch:
```
kubectl get events --watch &
kubectl scale deployment loggingdemo --replicas=5
```
break out of Watch:
```
fg
ctrl+c
```
checkout kube-system namespace:
```
kubectl get events --namespace kube-system
```
events are available in describe:
```
kubectl describe deployment nginx
kubectl describe replicaset nginx-xxxxx  # Update to your replicaset name
kubectl describe pods nginx
```
clean up:
```
kubectl delete -f multicontainer.yaml
kubectl delete deployment nginx
```
Note: Watch menu options in VSC.

---

#### <font color='red'> Logging Tools </font>
check out the following logging tools:
* EFK stack - Elasticsearch + FluentD + Kibana
* kubebox  - view POD logs from selected namespace
* kubetail - aggregate logging from multiple PODs
* kubewatch - publishes events to Slack


#### <font color='red'> EFK Stack </font>
ELK stands for Elasticsearch, Logstash, and Kibana. ELK is one of the popular log management platform used worldwide for log analysis. In the ELK stack, Logstash extracts the logging data or other events from different input sources. It processes the events and later stores them in Elasticsearch.


deploy EFK stack:
```
minikube addons open efk
```
to access Kibana:
```
kubectl get pods,svc -n kube-system
```
or 

> in browser: http://192.168.49.2:30003/

* select @timestamp
* click on menu -> Discover

#### <font color='red'> kubebox </font>
Kubebox is a terminal console for Kubernetes cluster which allows you to manage and monitor your cluster-live status with nice, old-school interface. Kubebox shows your pod resource usage, cluster monitoring, and container logs, etc. Additionally, you can easily navigate to the desired namespace and execute into the desired container for fast troubleshooting/recovery.

Link: https://github.com/astefanutti/kubebox


#### <font color='red'> kubetail </font>
Kubetail is a small bash script which allows you to aggregate logs from multiple pods into one stream. The initial Kubetail version doesn’t have filtering or highlighting features, but there is an additional Kubetail fork on Github. This can form and perform logs coloring using multitail tools.

Link: https://github.com/johanhaleby/kubetailhttps://github.com/aks/kubetail


#### <font color='red'> kubewatch </font>
Kubewatch is a Kubernetes watcher which can publish K8s events to the team communication app, Slack. Kubewatch runs as a pod inside Kubernetes clusters and monitors changes that occur in the system. You can specify the notifications you want to receive by editing the configuration file.

Link: https://github.com/bitnami-labs/kubewatch


