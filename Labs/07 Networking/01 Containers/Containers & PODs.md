## <font color='red'> 7.1 Networking - Containers </font>

In this lab we're going to examine the Kubernetes Network Model:
* 
* Shared resources
* Isolated resources
* Process isolation
* Networking

---

#### <font color='red'> 7.1.2 Container - Networking </font>
check whats running:
```
kubectl get all
```
deploy 1 container in a POD:
```
kubectl create -f 01_pod-single-container.yaml --save-config
```


#### <font color='red'> 7.1.2 Containers </font>
check whats running:
```
kubectl get all
```
deploy 2 containers in a POD:
```
kubectl create -f 01_pod-multiple-containers.yaml --save-config
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
lets exit:
```
exit
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

lets exit:
```
exit
```
network isolation c1:
```
kubectl exec -it podtest -c c1 -- ip link
```
Note: MAC address  

network isolation c2:
```
kubectl exec -it podtest -c c2 -- ip link
```
Note: both containers have the device: eth0 and MAC addresses.  

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





lets exit:
```
exit
```

---

cleanup:
```
kubectl delete -f 01_pod-multiple-containers.yaml
```

---