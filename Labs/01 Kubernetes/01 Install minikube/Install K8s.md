## <font color='red'> 1.1 K8s Cluster </font>
minikube is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes.

In this lab we're going to:
* Start minikube
* Tour of VS extensions

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
check minikube status:
```
minikube status
```

view addons:
```
minikube addons list
```

For additional insight into your cluster state, minikube bundles the Kubernetes Dashboard:
in a new terminal access dashboard:
```
minikube dashboard
```
> For further information: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

> Blog: https://www.replex.io/blog/how-to-install-access-and-add-heapster-metrics-to-the-kubernetes-dashboard

--- 

additional minikube commands.  

upgrade cluster:
```
minikube start --kubernetes-version=latest
```

> Minikube commands: https://minikube.sigs.k8s.io/docs/commands/

---