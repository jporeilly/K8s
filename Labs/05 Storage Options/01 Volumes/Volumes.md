## <font color='red'> 5.1 Volumes </font>
A Container's file system lives only as long as the Container does. So when a Container terminates and restarts, filesystem changes are lost. For more consistent storage that is independent of the Container, you can use a Volume. This is especially important for stateful applications, such as key-value stores (such as Redis) and databases.

In this Lab were going to cover Volumes:
* emptyDir {}


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

#### <font color='red'> 5.1.1 Volumes - emptyDir {} </font>
An emptyDir volume is an empty directory created in the pod. Containers can read and write files in this directory. The directory's contents are removed once the pod is deleted.

check whats running:
```
kubectl get all
```
install emptyDir volume:
```
kubectl create -f 01_volume-emptyDir.yaml --save-config
```
check whats running and watch for changes:
```
kubectl get pod redis --watch
```
in another terminal:
```
kubectl exec -it redis -- /bin/bash
```
go to /data/redis, and then create a file:
```
cd /data/redis/
echo Hello > test-file
```
list the running processes:
```
apt-get update
apt-get install procps
ps aux
```
kill the Redis process:
```
kill <pid> # where <pid> is the Redis process ID (PID).
```
Notice: the Container has terminated and restarted. This is because the Redis Pod has a restartPolicy of Always

shell back into container:
```
kubectl exec -it redis -- /bin/bash
```
In your shell, go to /data/redis, and verify that test-file is still there:
```
cd /data/redis/
ls -l
```
exit the shell:
```
exit
```
delete redis POD:
```
kubectl delete pod redis
```

---