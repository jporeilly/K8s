## <font color='red'> 8.1 Maintainance </font>
In this lab we're going to run through some common maintainace proceedures:
* Install etcdctl
* Authenticate access to etcd

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

#### <font color='red'> 8.1.2 Authenticate access to etcd </font>
check the PODs:
```
kubectl get pods --all-namesapces
```
find out about etcd-minikube:
```
kubectl describe pod etcd-minikube -n kube-system
```
copy certificates from **kube-apiserver** container and inject them into **Etcd** container:
```
kubectl cp --namespace kube-system kube-apiserver-minikube:var/lib/minikube/certs/apiserver-etcd-client.crt apiserver-etcd-client.crt  

kubectl cp --namespace kube-system apiserver-etcd-client.crt etcd-minikube:var/lib/minikube/certs/  

kubectl cp --namespace kube-system kube-apiserver-minikube:var/lib/minikube/certs/apiserver-etcd-client.key apiserver-etcd-client.key  

kubectl cp --namespace kube-system apiserver-etcd-client.key etcd-minikube:var/lib/minikube/certs/  
```

exec into etcd-minikube container:
```
kubectl exec -it etcd-minikube -n kube-system -- bin/sh
```
set etcdctl v3 API version:
```
export ETCDCTL_API=3
```
check keys:
```
etcdctl --cacert="etcd/ca.crt" --key=apiserver-etcd-client.key --cert=apiserver-etcd-client.crt get /registry/deployments --prefix --keys-only
```
Note: Looks weird but dont worry about it..!

```