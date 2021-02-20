## <font color='red'> 8.1 Maintainance </font>
In this lab we're going to run through some common maintainace proceedures:
* Install etcdctl


 #### ** Note: This Lab is just for reference. etcdctl has been installed.
---

#### <font color='red'> 8.1.1 Install etcdctl </font>
Installs the correct version of etcdctl.
Pre-requisites:
 * Install using a Terminal 
 * Make sure your in the Downloads directory.  

determine etcd version:
```
kubectl describe pod etcd-minikube -n kube-system
```
Note the etcd version.  

get the correct version:
```
kubectl exec -it etcd-minikube -n kube-system -- /bin/sh -c 'ETCDCTL_API=3 /usr/local/bin/etcd --version' | head
```
set variable:
```
export RELEASE="3.X.XX"
```
pull from GitHub:
```
wget https://github.com/etcd-io/etcd/releases/download/v${RELEASE}/etcd-v${RELEASE}-linux-amd64.tar.gz
```
next explode:
```
tar -zxvf etcd-v${RELEASE}-linux-amd64.tar.gz
```
change directory:
```
cd etcd-v${RELEASE}-linux-amd64
```
copy to local, so its in the PATH:
```
sudo cp etcdctl /usr/local/bin
```
quick test:
```
etcdctl --help | head 
```

---
