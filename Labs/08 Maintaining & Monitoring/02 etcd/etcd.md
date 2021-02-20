## <font color='red'> 8.2 Maintainance </font>
In this lab we're going to run through some common maintainace proceedures:
* Describe etcd
* Test process with a secret
* Backup and restore etcd

---

#### <font color='red'> 8.2.1 Describe etcd </font>
check out some of the key etcd configuration information
container image and tag, command, --data dir, and mounts and volumes for both etcd-certs and etcd-data

```
kubectl describe pod etcd-minikube -n kube-system
```
the configuration for etcd comes from the static pod manifest, check out the listen-client-urls, data-dir, volumeMounts, volumes/
Note: this command is for reference, as we're using Docker image minikube.
```
sudo more /etc/kubernetes/manifests/etcd.yaml
```
can also use:
```
ps -aux | grep etcd
```

---

#### <font color='red'> 8.2.1 Test etcd Backup & Restore </font>
create create a secret:
```
kubectl create secret generic test-secret \
    --from-literal=username='svcaccount' \
    --from-literal=password='S0mthingS0Str0ng!'
```
Note: from the describe the client endpoint: --listen-client-urls=https://127.0.0.1:2379,https://192.168.49.2:2379 

define a variable for the endpoint to etcd
```
ENDPOINThttps://192.168.49.2:2379
```
connect to the correct cluster & validate;
```
sudo ETCDCTL_API=3 etcdctl --endpoints $ENDPOINT \
--cacert /var/lib/minikube/certs/etcd/ca.crt \
--cert /var/lib/minikube/certs/etcd/server.crt \ 
--key /var/lib/minikube/certs/etcd/server.key \ 
member list
```

