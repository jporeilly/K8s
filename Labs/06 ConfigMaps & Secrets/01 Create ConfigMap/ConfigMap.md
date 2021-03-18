## <font color='red'> 6.1 ConfigMaps </font>
Kubernetes Secrets and ConfigMaps separate the configuration of individual container instances from the container image, reducing overhead and adding flexibility.  
Kubernetes has two types of objects that can inject configuration data into a container when it starts up: 
* ConfigMaps 
* Secrets


ConfigMaps and Secrets behave similarly in Kubernetes, both in how they are created and because they can be exposed inside a container as mounted files or volumes or environment variables.

In this lab were going to cover:
* ConfigMaps: store non-confidential data
  - environmental variables
  - volumes

---

#### <font color='red'>IMPORTANT:</font> 
<strong>Please ensure you start with a clean environment. 
If you have previously run minikube, you will need to delete the existing instance.</strong>

to stop  minikube:
```
minikube stop
```
to delete  minikube:
```
minikube delete
```
start minikube:
```
minikube start
```
start tunnel:
```
minikube tunnel
```

---

#### <font color='red'>6.1.1. configMaps</font> 
check whats running:
```
kubectl get all
```
deploy configMap:
```
kubectl apply -f 01_configmap.yaml
```
check configmap:
```
kubectl describe configmaps mongodb-configmap
```

**Env variables**

deploy app:
```
kubectl apply -f 02_pod-configmap-env.yaml
```
check Pods:
```
kubectl get pods
```
access the env:
```
kubectl exec -it pod-configmap-env --container mongodb -- /bin/bash
```
check env:
```
env
```
exit :
```
exit
```

**Volume**

deploy app:
```
kubectl apply -f 03_pod-configmap-vol.yaml
```
check Pods:
```
kubectl get pods
```
access the volume:
```
kubectl exec -it pod-configmap-vol --container mongodb -- /bin/bash
```
check volume:
```
ls /etc/config/
```
view data in volume:
```
cat /etc/config/*
```

clean up:
```
kubectl delete -f 01_configmap.yaml
kubectl delete -f 02_pod-configmap-env.yaml
kubectl delete -f 03_pod-configmap-vol.yaml
```

---