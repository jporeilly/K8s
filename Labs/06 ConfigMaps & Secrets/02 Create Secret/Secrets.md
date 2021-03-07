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
deploy app:
```
kubectl apply -f 02_pod-secrets-vol.yaml
```
check Pods:
```
kubectl get pods
```
access the volume:
```
kubectl exec -it pod-secrets-vol --container mongodb -- /bin/bash
```
check volume:
```
ls /etc/secret/
```
view data in volume:
```
cat /etc/secret/*
```

clean up:
```
kubectl delete -f 01_secrets.yaml
kubectl delete -f 02_pod-secrets-vol.yaml
```

---