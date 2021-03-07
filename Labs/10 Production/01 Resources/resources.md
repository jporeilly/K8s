## <font color='red'> 11.1 Resources </font>
When you specify a Pod, you can optionally specify how much of each resource a Container needs. The most common resources to specify are CPU and memory (RAM); there are others.

When you specify the resource request for Containers in a Pod, the scheduler uses this information to decide which node to place the Pod on. When you specify a resource limit for a Container, the kubelet enforces those limits so that the running container is not allowed to use more of that resource than the limit you set. The kubelet also reserves at least the request amount of that system resource specifically for that container to use 

In this lab we're going to:
* set resource quotas for namespaces
* Readiness
* Startup


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

#### <font color='red'> 11.1.1 Namespace Resources </font>
Set quotas for the total amount memory and CPU that can be used by all Containers running in a namespace.


check whats running:
```
kubectl get all
```
create a namespace:
```
kubectl create namespace quota-mem-cpu-example
```
deploy quota:
```
kubectl create -f 01_quota-mem-cpu.yaml --namespace=quota-mem-cpu-example  --save-config
```
Notice: healthy POD as Probe returns 0  

view info:
```
kubectl get resourcequota mem-cpu-demo --namespace=quota-mem-cpu-example --output=yaml
```
Notice: The ResourceQuota places these requirements on the quota-mem-cpu-example namespace:
* Every Container must have a memory request, memory limit, cpu request, and cpu limit.
* The memory request total for all Containers must not exceed 1 GiB.
* The memory limit total for all Containers must not exceed 2 GiB.
* The CPU request total for all Containers must not exceed 1 cpu.
* The CPU limit total for all Containers must not exceed 2 cpu.d 

get Pod details:
```
kubectl get pod quota-mem-cpu-demo --namespace=quota-mem-cpu-example
```
view info:
```
kubectl get resourcequota mem-cpu-demo --namespace=quota-mem-cpu-example --output=yaml
```
Notice: The output shows the quota along with how much of the quota has been used.

deploy a 2nd Pod:
```
kubectl create -f 02_quota-mem-cpu-pod-2.yaml --namespace=quota-mem-cpu-example
```
Note: The second Pod does not get created. 

---



kubectl delete namespace quota-mem-cpu-example



---