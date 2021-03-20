## <font color='red'> 7.1 Networking - Containers </font>
Networking is a central part of Kubernetes, but it can be challenging to understand exactly how it is expected to work. There are 4 distinct networking problems to address:

* Highly-coupled container-to-container communications: this is solved by Pods and localhost communications.
* Pod-to-Pod communications: this is the primary focus of this document.
* Pod-to-Service communications: this is covered in Services module.
* External-to-Service communications: this is covered in Services module.


In this lab we're going to examine the Container / Pods Kubernetes Network Model:
* Containers
* Shared resources
* Networking
* Isolated resources
* Process isolation

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


#### <font color='red'> 7.1.1 Networking - Containers </font>
check whats running:
```
kubectl get all
```
deploy 2 containers in a POD:
```
kubectl create -f 01_pod-multiple-containers.yaml --save-config
```
lets take a look at the containers:
```
eval $(minikube docker-env)
```
list our Containers + pause:
```
docker container ls | grep 'busybox\|pause'
```
connect to c1:
```
kubectl exec -it pod-multiple-containers -c c1 -- bin/sh
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
list our directories:
```
ls -l
```
change to tmp directory:
```
cd tmp
ls -l
```
Notice: we can view foo..


and lets take a look at the shared volume:
```
cat /shared/bar
```
Notice: we can view bar..

lets exit:
```
exit
```
connect to c2:
```
kubectl exec -it pod-multiple-containers -c c2 -- bin/sh
```
access files:
```
cat /shared/bar
cat /tmp/foo
```
Note: you can see file in /shared directory but not /tmp as filesystems are isolated from each other.  

lets exit:
```
exit
```

network isolation c1:
```
kubectl exec -it pod-multiple-containers -c c1 -- ip link
```
Note: MAC address  

network isolation c2:
```
kubectl exec -it pod-multiple-containers -c c2 -- ip link
```
Note: both containers have the device: eth0 and MAC addresses.  

connect to c1:
```
kubectl exec -it pod-multiple-containers -c c1 -- bin/sh
```
start network listener:
```
nc -lk -p 5000 127.0.0.1 -e 'date'
```
check processes:
```
ps -a
```
in a new terminal:
```
kubectl exec -it pod-multiple-containers -c c2 -- bin/sh
```
telnet to c1:
```
telnet localhost:5000
```
check processes:
```
ps -a
```
Note: cant see nc process but can see date.  So containers have process isolation but not network.

lets exit:
```
exit
```
cleanup:
```
kubectl delete -f 01_pod-multiple-containers.yaml
```

---

#### <font color='red'> 7.1.2 Container / Pod - Networking </font>
deploy 1 Container in a POD:
```
kubectl create -f 02_pod-single-container.yaml --save-config
```
lets take a look at the containers:
```
eval $(minikube docker-env)
```
list our Containers + pause:
```
docker container ls | grep 'busybox\|pause'
```

inspect the busybox Container networking:
```
docker inspect  [busybox Container ID]  --format='{{json .NetworkSettings}}'
```
Note: nothing..!


inspect the busybox Container networking:
```
docker inspect  [pause Container ID]  --format='{{json .NetworkSettings}}'
```
Notice: its the pause container thats handling the networking..

get the Pod info:
```
kubectl get pod -o wide
```
shell into container:
```
kubectl exec -it pod-single-container-xxxxx  --bin/sh
```
view the binding bwteen container on loopback and Pod eth0:
```
ip addr show
```
and exit:
```
exit
```
view the bridges:
```
brctl show virbr0
```


cleanup:
```
kubectl delete -f 02_pod-single-container.yaml
```

---