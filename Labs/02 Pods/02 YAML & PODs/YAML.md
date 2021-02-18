## <font color='red'> 2.2 YAML & PODs </font>

In this lab we're going to:
* Deploy an application using YAML  create/apply
* Run through some common kubectl commands
* Delete POD

---

#### <font color='red'> 2.2.1 YAML & PODs </font>
check whats running:
```
kubectl get all
```
test run and validate YAML:
```
kubectl create -f 01_nginx-pod.yaml --dry-run='client' --validate='true'
```
create nginx deployment:
```
kubectl create -f 01_nginx-pod.yaml --save-config
```
Notice: stores the configuration in annotations  

repeat nginx deployment:
```
kubectl create -f 01_nginx-pod.yaml 
```

check the YAML:
```
kubectl get pod nginx -o yaml
```
Notice: annotations    

can use edit / patch for non-disruptive changes
```
kubectl edit -f 01_nginx-pod.yaml -o yaml
```
use apply command:
```
kubectl apply -f 01_nginx-pod.yaml
```

---


#### <font color='red'> 2.2.2 Deleting PODs </font>
to delete the POD:
```
kubectl delete pod nginx
```
can also use:
```
kubectl delete -f 01_nginx-pod.yaml
```  

---

#### <font color='red'> 2.2.3 Deployment YAML & PODs </font>
check whats running:
```
kubectl get all
```
create nginx deployment:
```
kubectl create -f 02_nginx-deployment.yaml
```
then issue the command again:
```
kubectl create -f 02_nginx-deployment.yaml
```
Notice: an error will occur as the POD already exisits.

use apply command:
```
kubectl apply -f 02_nginx-deployment.yaml
```
Notice: Declarative as state is applied to resource.  If doesnt exist then created.

check the YAML:
```
kubectl get pod nginx-xxxxxx -o yaml
```
---

#### <font color='red'> 2.2.4 Delete Deployment YAML & PODs </font>
check whats running:
```
kubectl get all
```

to delete the POD:
```
kubectl delete pod nginx-xxxxx
```
Notice: the POD is deleted but another is created.
Note: the reason why you can delete the POD is that kind: POD is Deployment.

to delete:
```
kubectl delete -f 01_nginx.pod.yaml
``` 
or 
```
kubectl delete deployment.apps/nginx-deployment 
```

> YAML spec: https://yaml.org/spec/1.2/spec.html

---