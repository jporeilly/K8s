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

#### <font color='red'> 3.1.2 POD Liveness - http requests</font>
* initial 3s
* probe every 3s
* after 10s http request - 500 status
* POD restart  

check whats running:
```
kubectl get all
```
deploy liveness-http:
```
kubectl create -f 02_liveness-http.yaml --save-config
```
after 10s describe:
```
kubectl describe pod liveness-http
```
Notice: after 10s POD fails and has restarted 

clean up (force delete):
```
kubectl delete pods liveness-http --grace-period=0 --force
```

---

#### <font color='red'> 3.1.3 POD Liveness - tcp</font>
* initial 2s
* probe every 2s
* successThreshold = 1
* failureThreshold = 3
* POD restart  

check whats running:
```
kubectl get all
```
deploy liveness-tcp:
```
kubectl create -f 03_liveness-tcp.yaml --save-config
```
after 2s describe:
```
kubectl describe pod liveness-tcpsocket
```
Notice: POD continually fails as monitoring closed port 8888. Restart POD after 3 failed probe attempts.

clean up:
```
kubectl delete pods liveness-tcpsocket --grace-period=0 --force
```

---

#### <font color='red'> 3.1.4 POD Readiness - exec</font>
* initial 5s
* probe every 5s
* exec after 30s - fail  
* POD restart 

check whats running:
```
kubectl get all
```
deploy readiness-exec:
```
kubectl create -f 04_readiness-exec.yaml --save-config
```
within 30s describe:
```
kubectl describe pod readiness-exec
```
Notice: healthy POD as Probe returns 0  

after 30s check describe again:
```
kubectl describe pod readiness-exec
```
Notice: POD fails and has restarted 

wait another 30s:
```
kubectl get pod readiness-exec
```
Notice: Restarts incremented

clean up (force delete):
```
kubectl delete pods readiness-exec --grace-period=0 --force
```

---

#### <font color='red'> 3.1.5 POD Readiness - http requests</font>
* initial 3s
* probe every 3s
* after 10s http request - 500 status
* POD restart  

check whats running:
```
kubectl get all
```
deploy readiness-http:
```
kubectl create -f 05_readiness-http.yaml --save-config
```
after 10s describe:
```
kubectl describe pod readiness-http
```
Notice: after 10s POD fails and has restarted 

clean up (force delete):
```
kubectl delete pods readiness-http --grace-period=0 --force
```

---

#### <font color='red'> 3.1.6 POD Liveness - Startup - http</font>
* startup probe 75s
* probe every 10s
* liveness probe 1 failure threshold
* probe every 10s  

check whats running:
```
kubectl get all
```
deploy liveness-startup-http:
```
kubectl create -f 06_liveness-startup-http.yaml --save-config
```
after 10s describe:
```
kubectl describe pod liveness-startup-http
```
clean up (force delete):
```
kubectl delete pods liveness-startup-http --grace-period=0 --force
```

---

#### <font color='red'> 3.1.7 POD Liveness - Readiness - tcp</font>
* readiness intial 5s
* probe every 10s
* liveness initial 15s
* probe every 20s  

check whats running:
```
kubectl get all
```
deploy liveness-readiness-tcpp:
```
kubectl create -f 07_liveness-readiness-tcp.yaml --save-config
```
after 15s describe:
```
kubectl describe pod liveness-readiness-tcp
```
clean up (force delete):
```
kubectl delete pods liveness-readiness-tcp --grace-period=0 --force
```

---