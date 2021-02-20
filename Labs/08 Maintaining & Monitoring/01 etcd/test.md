## <font color='red'> 8.1 Maintainance </font>

In this lab we're going to run through some common maintainace proceedures:
* Backup and restore etcd
* POD -> POD
* POD -> Service
* External -> Service
* Multiple Clusters

---

#### <font color='red'> 8.1.1 ETCD Backup & Restore </font>
check out some of the key etcd configuration information
container image and tag, command, --data dir, and mounts and volumes for both etcd-certs and etcd-data

Note: select kube-system namespace
```
kubectl describe pod etcd-minikube -n kube-system
```
the configuration for etcd comes from the static pod manifest, check out the listen-client-urls, data-dir, volumeMounts, volumes/
```
sudo more var/lib/minikube/manifests/etcd.yaml
```