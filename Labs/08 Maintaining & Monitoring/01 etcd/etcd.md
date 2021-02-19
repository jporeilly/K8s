## <font color='red'> 8.1 Maintainance </font>

In this lab we're going to run through some common maintainace proceedures:
* Install etcdctl
* Backup and restore etcd
* Cluster Upgrade


---

#### <font color='red'> 8.1.2 Install etcdctl </font>



#### <font color='red'> 8.1.2 etcd Backup & Restore </font>
Note: select kube-system namespace

check out some of the key etcd configuration information:
container image and tag, command, --data dir, and mounts and volumes for both etcd-certs and etcd-data
```
kubectl describe pod etcd-minikube -n kube-system
```
the configuration for etcd comes from the static pod manifest, check out the listen-client-urls, data-dir, volumeMounts, volumes/
```
sudo more var/lib/minikube/manifests/etcd.yaml
```
runtime values:
```
ps -aux | grep etcd
```