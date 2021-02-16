## <font color='red'> 3.1 POD Helath </font>

In this lab we're going to:
* Deploy an application using YAML  create/apply
* Run through some common kubectl commands
* Delete POD

---

#### <font color='red'> 3.1.1 POD Liveness </font>
check whats running:
```
kubectl get all
```
test run and validate YAML:
```
kubectl create -f 01_nginx.pod.yaml --dry-run='client' --validate='true'
```
create nginx deployment:
```
kubectl create -f 01_nginx.pod.yaml --save-config
```
Notice: stores the configuration in annotations  

check the YAML:
```
kubectl get pod nginx-deployment-xxxxx -o yaml
```
Notice: annotations    

use apply command:
```
kubectl apply -f 01_nginx.pod.yaml
```
Notice: Declarative as state is applied to resource.  If doesnt exist then created.

can use edit / patch for non-disruptive changes
```
kubectl edit -f 01_nginx.pod.yaml -o yaml
```

---


#### <font color='red'> 3.1.2 POD Readiness </font>