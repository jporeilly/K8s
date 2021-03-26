## <font color='red'> 11.1 Resources </font>
When you specify a Pod, you can optionally specify how much of each resource a Container needs. The most common resources to specify are CPU and memory (RAM); there are others.

When you specify the resource request for Containers in a Pod, the scheduler uses this information to decide which node to place the Pod on. When you specify a resource limit for a Container, the kubelet enforces those limits so that the running container is not allowed to use more of that resource than the limit you set. The kubelet also reserves at least the request amount of that system resource specifically for that container to use 

In this lab we're going to:
* set resources for cluter
* set resource quotas for namespaces
* VPA - Vertical Pod Allocation


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
enable metrics-server:
```
minikube addons enable metrics-server
```
start tunnel:
```
minikube tunnel
```

--- 

#### <font color='red'> 11.1.1 Pod Resources </font>

create a quota-test namespace:
```
kubectl create namespace quota-test
```
deploy cpu-quota:
```
kubectl apply -f 01_cpu-quota.yaml
```
Note: for cpu request=100m cpu with a limit=200m

verify quota was applied:
```
kubectl describe resourcequota/test-cpu-quota --namespace quota-test
```

to test deploy 3 Pods:

deploy Pod-A
```
kubectl create -f 02_Pod-A.yaml --save-config
```
Note: Pod-A cpu request=50m  limit=100m. So 50% cpu has been allocated to Pod-A.

verify quota was applied:
```
kubectl describe resourcequota/test-cpu-quota --namespace quota-test
```
in another terminal run:
```
kubectl top pods
```

deploy Pod-B
```
kubectl create -f 03_Pod-B.yaml --save-config
```
Note: Pod-A 50% cpu Pod-B 50%.

verify quota was applied:
```
kubectl describe resourcequota/test-cpu-quota --namespace quota-test
```
in another terminal run:
```
kubectl top pods
```

deploy Pod-C
```
kubectl create -f 04_Pod-C.yaml --save-config
```
Note: Pod-A 50% cpu Pod-B 50% Pod-C

verify quota was applied:
```
kubectl describe resourcequota/test-cpu-quota --namespace quota-test
```
in another terminal run:
```
kubectl top pods
```


Note: From the output you can see that the memory utilised is 64Mi and the total CPU used is 458m.
Notice: The current values for CPU and memory are greater than the requests that you defined earlier (cpu=50m,memory=50Mi).

However, why is the container consuming only 458 millicores?
Since the Pod is running an infinite loop, you might expect it to consume 100% of the available CPU (or 1000 millicores).  
Why is it not running at 100% CPU?  
When you define a CPU request in Kubernetes, that doesn't only describe the minimum amount of CPU but also establishes a share of CPU for that container.
All containers share the same CPU, but they are nice to each other, and split the times based on their shares.




cleanup:
```
docker rm containerstack/cpustress
docker prune -a
```

---

#### <font color='red'> 11.1. VPA Vertical Pod Autoscaler </font>





---