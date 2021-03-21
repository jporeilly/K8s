## <font color='red'> 10.1 Troubleshooting </font>
Sometimes things go wrong. This guide is aimed at making them right. It has two sections:

* Troubleshooting your application
Useful for users who are deploying code into Kubernetes and wondering why it is not working.

* Troubleshooting your cluster 
Useful for cluster administrators and people whose Kubernetes cluster is unhappy.
You should also check the known issues for the release you're using.  

**There are no videos with this section its intended to just be a guide**

---

#### <font color='red'> Troubleshooting Applications </font>

Diagnosing the problem
The first step in troubleshooting is triage. What is the problem? 
Is it your Pods, your Replication Controller or your Service?

* Debugging Pods
* debugging Running Pods
* Debugging Replication Controllers
* Debugging Services

---

**Debugging Pods**


check the current state of the Pod:
```
kubectl describe pods ${POD_NAME}
```


* Pending
If a Pod is stuck in Pending it means that it can not be scheduled onto a node. Generally this is because there are insufficient resources of one type or another that prevent scheduling.

* Waiting
If a Pod is stuck in the Waiting state, then it has been scheduled to a worker node, but it can't run on that machine. The most common cause of Waiting pods is a failure to pull the image. 


There are three things to check:
- Make sure that you have the name of the image correct.
- Have you pushed the image to the repository?
- Run a manual docker pull <image> on your machine to see if the image can be pulled


If your pod is not behaving as you expected, it may be that there was an error in your pod description.

validate the yaml. Delete the Pod and rerun with --validate flag:
```
kubectl apply --validate -f [manifest.yaml] 
```
check is whether the Pod on the apiserver matches the Pod create:
```
kubectl get pods/[mypod] -o yaml > [mypod]-on-apiserver.yaml
```

---

**Debugging Running Pods**


---

**Debugging Replication Controllers**
Replication controllers are fairly straightforward. They can either create Pods or they can't. 

---

**Debugging Services**



---