## <font color='red'> 4.2 Discovery </font>

check whats running:
```
kubectl get all
```
install my-nginx:
```
kubectl run my-nginx --image=nginx:alpine
```
check whats running:
```
kubectl get all
```
POD_NAME=$(kubectl get pod \
    --no-headers \
    -o=custom-columns=NAME:.metadata.name \
    -l type=api,service=go-demo-2 \
    | tail -1)

kubectl exec $POD_NAME env

delete [pod-name]:
```
kubectl delete pod [pod-name]
```
check PODs:
```
kubectl get pods
```
> Notice new POD created

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