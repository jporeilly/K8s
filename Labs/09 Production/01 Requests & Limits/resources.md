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
minikube start --nodes 3 -p multinode-demo
```
start tunnel:
```
minikube tunnel
```

--- 

#### <font color='red'> 11.1.1 Pod Resources </font>
run a busybox and request 50m cpu and memory 50:
```
kubectl run -i --tty --rm busybox \
  --image=busybox \
  --restart=Never \
  --requests='cpu=50m,memory=50Mi' -- sh
```
fill the memory:
```
dd if=/dev/zero of=/dev/shm/fill bs=1k count=1024k
```
increase cpu:
```
while true; do true; done
```
in another terminal run:
```
kubectl top pods
```
Note: From the output you can see that the memory utilised is 64Mi and the total CPU used is 462m.
Notice: The current values for CPU and memory are greater than the requests that you defined earlier (cpu=50m,memory=50Mi).

However, why is the container consuming only 400 millicores?
Since the Pod is running an infinite loop, you might expect it to consume 100% of the available CPU (or 1000 millicores).
hy is it not running at 100% CPU?
en you define a CPU request in Kubernetes, that doesn't only describe the minimum amount of CPU but also establishes a share of CPU for that container.
All containers share the same CPU, but they are nice to each other, and they split the times based on their shares.

deteremine how many cores:
```
docker info | grep CPUs
```
run a container that consumes all available CPU and assign it a CPU share of 1024:
```
docker run -d --rm --name stresser-1024 \
  --cpu-shares 1024 \
  containerstack/cpustress --cpu 2
```
view resources consumed:
```
docker stats
```
Note: CPU percentage is the sum of the percentage per core

in a new terminal:
```
docker run -d --rm --name stresser-2048 \
  --cpu-shares 2048 \
  containerstack/cpustress --cpu 2
```
view resources consumed:
```
docker stats
```
Notice: When two containers are running in a 2 vCPU node, the stresser-2048 container gets twice the share of the available CPU.

in a new terminal:
```
docker run -d --name stresser-3072 \
  --cpu-shares 3072 \
  containerstack/cpustress --cpu 2
```
Notice: the split ratio...

---

#### <font color='red'> 11.1.2 Namespace Resources </font>
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

clean up

kubectl delete namespace quota-mem-cpu-example



---

#### <font color='red'> 11.1. VPA Vertical Pod Autoscaler </font>





---