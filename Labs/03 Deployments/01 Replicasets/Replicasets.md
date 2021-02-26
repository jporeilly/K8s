## <font color='red'> 3.1 Replicasets </font>
A ReplicaSet is defined with fields, including a selector that specifies how to identify Pods it can acquire, a number of replicas indicating how many Pods it should be maintaining, and a pod template specifying the data of new Pods it should create to meet the number of replicas criteria. A ReplicaSet then fulfills its purpose by creating and deleting Pods as needed to reach the desired number. When a ReplicaSet needs to create new Pods, it uses its Pod template.

In this lab we're going to examine:
* Replicasets

---

#### <font color='red'>IMPORTANT:</font> 
<strong>Please ensure you start with a clean environment. 
If you have previously run minikube, you will need to delete the existing instance.</strong>

to stop  minikube:
```
minikube stop
```
to delete  minikube:
```
minikube delete
```
start minikube:
```
minikube start
```
start tunnel:
```
minikube tunnel
```

--- 

#### <font color='red'> 3.1.1 Replicaset </font>
to access the kubectl commands:
```
kubectl
```
check whats running on Kubernetes:
```
kubectl get all
```
deploy replicaset:
```
kubectl create -f 01_nginx-replicaset.yaml
```
check PODs:
```
kubectl get pods
```
and 
```
kubectl get rs
```
check a POD to see if managed by replicaset:
```
kubectl get pods web-xxxx -o yaml | grep -A 5 owner
```

#### <font color='red'> 3.1.2 Remove a Replicaset </font>
edit a POD:
```
kubectl edit pods web-xxx
```
change pod label:
```
role=isolated
```
check PODs:
```
kubectl get pods
```
Notice: now have 5 PODs. the replicaset has created a new POD for web and still have 'isolated'.

---

#### <font color='red'> 3.1.3 Scaling Replicaset </font>
* edit controller replicaset
* use kubectl

edit controller config:
```
kubectl edit rs web
```
change # of replicas.  

set scale:
```
kubectl scale --replicas=2 rs/web
```
can also use autoscalers accorsing to cpu load:
```
kubectl autoscale rs web --max=5
```
Note: Uses Horizontal Pod Autoscaler (HPA)

** remember to set back to 4**

can also deploy a scaler:
```
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: web-scaler
spec:
  scaleTargetRef:
    kind: ReplicaSet
    name: web
  minReplicas: 2
  maxReplicas: 5
  targetCPUUtilizationPercentage: 50
```

---

#### <font color='red'> 3.1.4 Adopting PODs </font>
* uses Apache

deploy apache orphan:
```
kubectl create -f 02_apache-orphan.yaml --save-config
```
check PODs:
```
kubectl get pods
```
Notice: Orphan POD terminates. Its adopted by controller as label (web) matches nginx (web) label.

Note: if Orphan had been started first then a web POD would have been deleted.

---

#### <font color='red'> 3.1.5 Deleting Replicasets </font>
delete replicaset:
```
kubectl delete rs web
```
or use yaml:
```
kubectl delete -f 01_nginx-replicaset.yaml
kubectl delete -f 02_apache-orphan.yaml
```
to delete PODs but not replicaset:
```
kubectl delete rs [ReplicaSet name] --cascade=false
```

---