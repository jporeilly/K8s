## <font color='red'> 5.2 Persistent Volumes </font>
A persistent volume is a piece of storage in a cluster that an administrator has provisioned. It is a resource in the cluster, just as a node is a cluster resource. A persistent volume is a volume plug-in that has a lifecycle independent of any individual pod that uses the persistent volume. This API object captures details of implementation if the storage like NFS or a cloud-provider-specific storage system.

The workflow for creating a persistent volume
* A cluster administrator, create a PersistentVolume backed by physical storage. You do not associate the volume with any Pod.

* taking the role of a developer / cluster user, create a PersistentVolumeClaim that is automatically bound to a suitable PersistentVolume.

* create a Pod that uses the above PersistentVolumeClaim for storage.

---

#### <font color='red'> 5.2.1 Persistent Volumes Dir</font>

open a shell to Node:
```
minikube ssh
```
create a /mnt/data directory:
```
sudo mkdir /mnt/data
```
create an index.html file on the Node:
```
sudo sh -c "echo 'Hello from Kubernetes storage' > /mnt/data/index.html"
```
test that the index.html file exists:
```
cat /mnt/data/index.html
```

---

#### <font color='red'> 5.2.1 Persistent Volumes </font>

deploy:
```
kubectl apply -f 01_persistent-volume.yaml
```
view information about the PersistentVolume:
```
kubectl get pv task-pv-volume
```
Note: The output shows that the PersistentVolume has a STATUS of Available. This means it has not yet been bound to a PersistentVolumeClaim.

create a PVC:
```
kubectl apply -f 02_persistent-volume-claim.yaml
```
check pv:
```
kubectl get pv task-pv-volume
```
Note: output shows the staus of the bound

deploy nginx:
```
kubectl apply -f 03_nginx.yaml
```
check Pod:
```
kubectl get pod task-pv-pod
```
shell into Container:
```
kubectl exec -it task-pv-pod -- /bin/bash
```
verify that nginx is serving the index.html file from the hostPath volume:
```
apt update
apt install curl
curl http://localhost/
```
exit container:
```
exit
```
clean up:
```
kubectl delete pod task-pv-pod
kubectl delete pvc task-pv-claim
kubectl delete pv task-pv-volume
```
delete directories on Host:
```
sudo rm /mnt/data/index.html
sudo rmdir /mnt/data
```

---