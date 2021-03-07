## <font color='red'> 6.2 Secrets </font>
Kubernetes Secrets let you store and manage sensitive information, such as passwords, OAuth tokens, and ssh keys. Storing confidential information in a Secret is safer and more flexible than putting it verbatim in a Pod definition or in a container image.

A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. Such information might otherwise be put in a Pod specification or in an image. Users can create Secrets and the system also creates some Secrets.

In this Lab we're going to cover:
* create a key & token
* create a secret

---

check whats running:
```
kubectl get all
```
base64 encode our access key and API token:
```
echo "OUR_API_ACCESS_KEY" | base64
echo "SECRET_7t4836378erwdser34" | base64
```
deploy secrets:
```
kubectl apply -f 01_secrets.yaml
```


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