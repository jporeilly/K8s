## <font color='red'> 3.1 Replicasets </font>
A ReplicaSet is defined with fields, including a selector that specifies how to identify Pods it can acquire, a number of replicas indicating how many Pods it should be maintaining, and a pod template specifying the data of new Pods it should create to meet the number of replicas criteria. A ReplicaSet then fulfills its purpose by creating and deleting Pods as needed to reach the desired number. When a ReplicaSet needs to create new Pods, it uses its Pod template.

In this lab we're going to examine Replicasets:
* Removing
* Scaling
* Adopting
* Deleting

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
check replicaset:
```
kubectl get rs/nginx
```
check a POD to see if managed by replicaset:
```
kubectl get pods nginx-xxxx -o yaml | grep -A 5 owner
```

---

#### <font color='red'> 3.1.2 Scaling Replicaset </font>
* edit controller replicaset
* use kubectl

edit controller config:
```
kubectl edit rs nginx
```
change # of replicas to 4.  
set scale:
```
kubectl scale --replicas=4 rs/nginx
```
check Pods:
```
kubectl get pods
```
Notice: now have 4 Pods..

can also use autoscalers according to cpu load:
```
kubectl autoscale rs nginx --max=5
```
Note: Uses Horizontal Pod Autoscaler (HPA)

** remember to set back to 2**

delete exisiting HPA:
```
kubectl delete hpa nginx
```
then reset to 2:
```
kubectl autoscale rs nginx --max=2
```
Notice: Pods will start terminating as its scaled back..
```
kubectl get pods
```

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
optional..
to deploy scaler:
```
kubectl create -f 02_nginx-replicaset-scaler.yaml
```

---

#### <font color='red'> 3.1.3 Adopting PODs </font>
* deploy 2 Pods - not adopted
* matched on label, so Controller takes ownership

deploy apache orphan:
```
kubectl create -f 03_nginx-replicaset-adopted.yaml --save-config
```
check PODs:
```
kubectl get pods
```
Notice: Orphan POD terminates. Its adopted by controller as label (web) matches nginx (web) label.

Note: if Orphan had been started first then a web POD would have been deleted.

---

#### <font color='red'> 3.1.4 Deleting Replicasets </font>
delete replicaset:
```
kubectl delete rs nginx
```
or use yaml:
```
kubectl delete -f 01_nginx-replicaset.yaml --grace-period=0 --force
kubectl delete -f 02_nginx-replicaset-scaler.yaml --grace-period=0 --force    (if deployed)
kubectl delete -f 03_nginx-replicaset-adopted.yaml --grace-period=0 --force
```
to delete PODs but not replicaset:
```
kubectl delete rs [ReplicaSet name] --cascade=false
```

---