## <font color='red'> 2.1 YAML & PODs </font>

In this lab we're going to:
* Deploy an application using YAML  create/apply
* Run through some common kubectl commands
* Delete POD

---

#### <font color='red'> 2.1.1 YAML & PODs </font>
check whats running:
```
kubectl get all
```
try a trial run and validate YAML:
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
kubectl get pod nginx -o yaml
```
Notice: annotations    

use apply command:
```
kubectl apply -f 01_nginx.pod.yaml
```
Notice: Declarative as state is applied to resource.  If doesnt exist then created.

can use edit / patch for non-disruptive changes
```
kubectl edit -f nginx.pod.yaml -o yaml
```

to delete the POD:
```
kubectl delete pod nginx
```
can also use:
```
kubectl delete -f 01_nginx.pod.yaml
```  

---

#### <font color='red'> 2.1.2 Deployment YAML & PODs </font>
check whats running:
```
kubectl get all
```
create nginx deployment:
```
kubectl create -f 02_nginx.deployment.yaml --save-config
```
then issue the command again:
```
kubectl create –f 02_nginx.deployment.yaml
```
Notice: an error will occur as the POD already exisits.

use apply command:
```
kubectl apply -f 01_nginx.deployment.yaml
```
Notice: Declarative as state is applied to resource.  If doesnt exist then created.




> YAML spec: https://yaml.org/spec/1.2/spec.html


---