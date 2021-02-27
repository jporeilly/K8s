## <font color='red'> 5.2 Persistent Volumes </font>

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
for port-forward:
```
kubectl port-forward [pod-name] 8000:80
```
check PODs:
```
kubectl get pods
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