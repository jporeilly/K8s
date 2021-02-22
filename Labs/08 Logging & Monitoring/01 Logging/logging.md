## <font color='red'> 8.1 Logging </font>

In this lab we're going to examine Logging:
* Containers & PODs
* Nodes
* Control Plane
* Events

---

#### <font color='red'> 8.1.1 Logging Containers / PODs </font>
**single container**  

deploy a Nginx POD:
```
kubectl create deployment nginx --image=nginx
```
set the variable for the POD:
```
PODNAME=$(kubectl get pods -l app=nginx -o jsonpath='{ .items[0].metadata.name }')
```
check variable:
```
echo $PODNAME
```
access Nginx POD logs:
```
kubectl logs $PODNAME
```
clean up:
```
kubectl delete deployment nginx
```  

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

---

#### <font color='red'> 8.1.1 Logging Nodes </font>
kubelet status:
```
systemctl status kubelet.service
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

> checkout logviewr: http://192.168.49.2:32000/

---

#### <font color='red'> 8.1.2 Control Plane </font>
get a list of control plane PODs:
```
kubectl get pods --namespace kube-system --selector tier=control-plane
```
retrieve logs:
```
kubectl logs --namespace kube-system kube-apiserver-minikube
```
**Note: these commands are for a bare metal K8s implementation**

if control plane is down:
```
sudo docker ps
```
log for api-server:
```
sudo docker ps  | grep k8s_kube-apiserver
CONTAINER_ID=$(sudo docker ps | grep k8s_kube-apiserver | awk '{ print $1 }')
echo $CONTAINER_ID
sudo docker logs $CONTAINER_ID
```
and if Docker is down:
```
sudo ls /var/log/containers
sudo tail /var/log/containers/kube-apiserver-[master-container]*
```
Note: Logs menu options in VSC. 

---

#### <font color='red'> 8.1.3 Events </font>
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
* logviewer - streams POD logs from namespace