## <font color='red'> 1.1 K8s Cluster </font>

By default, kubectl gets configured to access the kubernetes cluster control plane inside minikube when the minikube start command is executed.

kubectl version:

```
kubectl version
```

check nodes:

```
kubectl get nodes
```

check everything (PODs & Services):

```
kubectl get all
```

check just PODs:

```
kubectl get pods
```

For kubectl commands: https://kubernetes.io/docs/reference/kubectl/cheatsheet/

<a href="./K8s Commands.docx" target="_top">K8s Commands</a>

---

## <font color='red'> 1.2 K8s Namespace </font>

Kubernetes supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called namespaces.

check namespaces:

```
kubectl get namespace
```

check kube-system namespace:

```
kubectl get pods -n kube-system
```

create a new namespace:

```
kubectl create namespace Lumada
```

now developers can deploy apps to the namespace you created:

delete namespace:

```
kubectl delete namespace Lumada
```

then you would check to make sure everything has been deleted:

```
kubectl get pods --all-namespaces
```
