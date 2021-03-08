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

lets scale back down:
```
kubectl scale --replicas=2 rs/nginx
```
clean up:
```
kubectl delete -f 01_nginx-replicaset.yaml
```

---

#### <font color='red'> 3.1.3 Scaling Replicaset - HPA </font>
can also use autoscalers according to cpu load:

enable metrics-server:
```
minikube addons enable metrics-server
```
deploy app + service + hpa
```
kubectl apply -f 02_hello-replicaset-hpa.yaml
```
deploy load-generator:
```
kubectl apply -f 03_load-generator.yaml
```
check metrics-server:
it will take a few minutes for the metrics-server to be deployed...
```
kubectl top nodes
kubectl top pods
```
Notice: resource usage of the nodes / pods .


check status of HPA:
```
kubectl describe hpa hello
```
check metrics-server:
```
kubectl get pods --namespace kube-system | grep metrics-server
```
resource usage for Pods:
```
kubectl top pods
```
at container level:
```
kubectl top pods --containers
```


you can apply some pressure to the cluster by scaling up the load-generator:
```
kubectl scale deployment/load-generator --replicas 2
```
and watch the Pods terminate as you decrease load-generator pressure:
```
kubectl scale deployment/load-generator --replicas 1
```
clean up:
```
kubectl delete -f 02_hello-replicaset-hpa.yaml
kubectl apply -f 03_load-generator.yaml
```
---

#### <font color='red'> 3.1.3 Adopting PODs </font>
* deploy 2 Pods - not adopted
* matched on label, so Controller takes ownership

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
deploy apache orphan:
```
kubectl create -f 04_nginx-replicaset-adopted.yaml --save-config
```
check PODs:
```
kubectl get pods
```
Notice: Orphan POD terminates. Its adopted by controller as label (web) matches nginx (web) label.

Note: if Orphan had been started first then a web POD would have been deleted.

clean up:
```
kubectl delete -f 01_nginx-replicaset.yaml --grace-period=0 --force
kubectl delete -f 04_nginx-replicaset-adopted.yaml --grace-period=0 --force
```
to delete PODs but not replicaset:
```
kubectl delete rs [ReplicaSet name] --cascade=false
```

---