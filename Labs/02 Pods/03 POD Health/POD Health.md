## <font color='red'> 3.1 POD Helath </font>

In this lab we're going to implement the following Probes:
* Liveness
* Readiness
* Startup

---

#### <font color='red'> 3.1.1 POD Liveness </font>
check whats running:
```
kubectl get all
```
deploy busybox:
```
kubectl create -f 01_busybox-liveness.yaml --save-config
```
within 30s:
```
kubectl describe pod liveness-exec
```
Notice: healthy POD as Probe returns 0  

after 30s describe again:
```
kubectl describe pod liveness-exec
```
Notice: POD fails and is restarted 

wait another 30s:
```
kubectl get pod liveness-exec
```
Notice: Restarts incremented

---


#### <font color='red'> 3.1.2 POD Liveness - http requests</font>
check whats running:
```
kubectl get all
```