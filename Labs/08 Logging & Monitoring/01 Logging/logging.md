## <font color='red'> 8.1 Logging </font>

In this lab we're going to examine Logging:
* Containers & PODs
* Nodes
* POD -> Service
* External -> Service
* Multiple Clusters

---

#### <font color='red'> 8.1.1 Logging Containers / PODs </font>
single container  

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

multi-container  

deploy busybox multi-container:
```  
kubectl create -f 01_multicontainer.yaml --save-config
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
kubectl logs --selector app=loggingdemo --all-containers --tail 5
```

---

#### <font color='red'> 8.1.1 Logging Nodes </font>
kubelet status:
```
systemctl status kubelet
```