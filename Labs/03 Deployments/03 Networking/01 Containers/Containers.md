## <font color='red'> 3.3 Networkin </font>

In this lab we're going to examine the Kubernetes Network Model:
* Container -> Container
* POD -> POD
* POD -> Service
* External -> Service
* Multiple Clusters

---

#### <font color='red'> 3.3.1 Containers </font>
check whats running:
```
kubectl get all
```
deploy 2 containers in a POD:
```
kubectl create -f 01_pod-multiple-containers.yaml
```
connect to c1:
```
kubectl exec -it podtest -c c1 -- sh
```
see the volume on c1:
```
mount | grep shared
```
Note: volume is mounted as 'shared'.  

create some files:
```
echo "foo" > tmp/foo  && echo "bar" > /shared/bar
```
connect to c2:
```
kubectl exec -it podtest -c c2 -- sh
```
access files:
```
cat /shared/bar
cat /tmp/foo
```
Note: you can see file in /shared directory but not /tmp as filesystems are isolated from each other.  

network isolation c1:
```
kubectl exec -it podtest -c c1 -- ip link
```
Note: MAC address  

network isolation c2:
```
kubectl exec -it podtest -c c2 -- ip link
```
Note: both containers ahve the device: eth0 and MAC addresses.  

connect to c1:
```
kubectl exec -it podtest -c c1 -- sh
```
start newtwork listener:
```
nc -lk -p 5000 127.0.0.1 -e 'date'
```
in a new terminal:
```
kubectl exec -it podtest -c c2 -- sh
```
telnet to c1:
```
telnet localhost:5000
```
Note: cant see nc process but can see date.  So containers have process isolation but not network.


---


#### <font color='red'> 2.2.2 Deleting PODs </font>
to delete the POD:
```
kubectl delete pod nginx
```
can also use:
```
kubectl delete -f 01_nginx-pod.yaml
```  

---

#### <font color='red'> 2.2.3 Deployment YAML & PODs </font>
check whats running:
```
kubectl get all
```
create nginx deployment:
```
kubectl create -f 02_nginx-deployment.yaml
```
then issue the command again:
```
kubectl create -f 02_nginx-deployment.yaml
```
Notice: an error will occur as the POD already exisits.

use apply command:
```
kubectl apply -f 02_nginx-deployment.yaml
```
Notice: Declarative as state is applied to resource.  If doesnt exist then created.

check the YAML:
```
kubectl get pod nginx-xxxxxx -o yaml
```
---

#### <font color='red'> 2.2.4 Delete Deployment YAML & PODs </font>
check whats running:
```
kubectl get all
```

to delete the POD:
```
kubectl delete pod nginx-xxxxx
```
Notice: the POD is deleted but another is created.
Note: the reason why you can delete the POD is that kind: POD is Deployment.

to delete:
```
kubectl delete -f 01_nginx.pod.yaml
``` 
or 
```
kubectl delete deployment.apps/nginx-deployment 
```

> YAML spec: https://yaml.org/spec/1.2/spec.html

---