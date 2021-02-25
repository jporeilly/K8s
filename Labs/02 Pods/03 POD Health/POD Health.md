## <font color='red'> 3.1 POD Helath </font>

In this lab we're going to implement the following Probes:
* Liveness
* Readiness
* Startup

with the 3 actions:
* exec
* http
* tcp

---

#### <font color='red'> 3.1.1 POD Liveness </font>
check whats running:
```
kubectl get all
```
deploy liveness-exec:
```
kubectl create -f 01_liveness-exec.yaml --save-config
```
within 30s describe:
```
kubectl describe pod liveness-exec
```
Notice: healthy POD as Probe returns 0  

after 30s check events again:
```
kubectl events pod liveness-exec
```
Notice: POD fails and has restarted 

wait another 30s:
```
kubectl get pod liveness-exec
```
Notice: Restarts incremented

clean up:
```
kubectl delete pod liveness-exec
```

---


#### <font color='red'> 3.1.2 POD Liveness - http requests</font>
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

clean up:
```
kubectl delete pod liveness-http
```

---


#### <font color='red'> 3.1.3 POD Liveness - tcp</font>
check whats running:
```
kubectl get all
```
deploy liveness-http:
```
kubectl create -f 03_liveness-tcp.yaml --save-config
```
after 10s check events again:
```
kubectl events pod liveness-http
```
Notice: after 10s POD fails and has restarted 

clean up:
```
kubectl delete pod liveness-http
```

---


#### <font color='red'> 3.1.3 POD Readiness - Start up</font>
check whats running:
```
kubectl get all
```
deploy liveness-http:
```
kubectl create -f 03_readiness-http.yaml --save-config
```


---


#### <font color='red'> 3.1.2 POD Liveness - Start up</font>
check whats running:
```
kubectl get all
```
deploy liveness-http:
```
kubectl create -f 02_liveness-http.yaml --save-config
```