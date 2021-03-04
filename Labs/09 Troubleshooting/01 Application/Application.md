## <font color='red'> 9.1 Troubleshooting Applications </font>
This guide is to help users debug applications that are deployed into Kubernetes and not behaving correctly.
The first step in troubleshooting is triage. What is the problem? Is it your Pods, your Replication Controller or your Service?

In this lab we're going to run through troubleshooting:
* Debugging Pods
* Debugging Replication Controllers
* Debugging Services

---

#### <font color='red'> 9.1.1 Debugging Pods </font>
The first step in debugging a Pod is taking a look at it:
```
kubectl describe pods ${POD_NAME}
```

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