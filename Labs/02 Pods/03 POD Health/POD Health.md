## <font color='red'> 3.1 POD Health </font>
Health checks are a simple way to let the system know if an instance of your app is working or not working. If an instance of your app is not working, then other services should not access it or send a request to it. Instead, requests should be sent to another instance of the app that is ready, or retried at a later time. The system should also bring your app back to a healthy state.  

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
after 20s describe:
```
kubectl describe pod liveness-readiness-tcp
```
clean up (force delete):
```
kubectl delete pods liveness-readiness-tcp --grace-period=0 --force
```

---