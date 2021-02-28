## <font color='red'> 5.2 Persistent Volumes </font>



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
create an index.html file:
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
check whats running:
```
kubectl get all
```
to delete a POD:
```
kubectl delete deployment my-nginx
```
check whats running:
```
kubectl get all
```
> Notice POD is Terminating  

set a Watch:
```
kubectl get pods --watch
```
---