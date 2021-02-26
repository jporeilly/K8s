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
* initial 5s
* probe every 5s
* exec after 30s - fail  
* POD restart  

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

after 30s check describe again:
```
kubectl describe pod liveness-exec
```
Notice: POD fails and has restarted 

wait another 30s:
```
kubectl get pod liveness-exec
```
Notice: Restarts incremented

clean up (force delete):
```
kubectl delete pods liveness-exec --grace-period=0 --force
```

---

#### <font color='red'> 3.1.2 POD Liveness - http requests</font>
* initial 5s
* probe every 5s
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

clean up:
```
kubectl delete pod liveness-http
```

---

#### <font color='red'> 3.1.3 POD Liveness - tcp</font>
* initial 15s
* probe every 20s
* all good after 15s  

check whats running:
```
kubectl get all
```
deploy liveness-tcp:
```
kubectl create -f 03_liveness-tcp.yaml --save-config
```
after 15s describe:
```
kubectl describe pod liveness-tcp
```
Notice: POD is good to accept tcp after 15s

clean up:
```
kubectl delete pod liveness-tcp
```

---


#### <font color='red'> 3.1.3 POD Readiness - Start up</font>
* initial 15s
* probe every 20s
* all good after 15s  

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