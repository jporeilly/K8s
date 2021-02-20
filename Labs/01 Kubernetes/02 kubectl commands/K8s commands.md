## <font color='red'> 1.2 K8s Cluster Commands</font>

By default, kubectl gets configured to access the kubernetes cluster control plane inside minikube when the minikube start command is executed.

kubectl version:
```
kubectl version
```
list kubectl commands:
```
kubectl
```
view cluster info:
```
kubectl cluster-info
```
check nodes:
```
kubectl get nodes -o wide
```
check everything (PODs & Services):
```
kubectl get all
```
check just PODs:
```
kubectl get pods
```
simple way to deploy a POD:
```
kubectl run [container-name] --image=[image-name]
```
port forward to allow external access:
```
kubectl port-forward [POD] [ports]
```
expose a port for deployment:
```
kubectl expose
```
create a resource:
```
kubectl create [resource]
```
create or modify resource:
```
kubectl apply [resource]
```

For kubectl commands: https://kubernetes.io/docs/reference/kubectl/cheatsheet/

Document: <a href="./K8s Commands.pdf" target="_top">K8s Commands</a>

---