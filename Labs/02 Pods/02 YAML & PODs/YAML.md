## <font color='red'> 2.2 YAML & PODs </font>

In this lab we're going to:
* Deploy an application
* Run through some common kubectl commands
* Delete POD

---

#### <font color='red'> 2.2.1 Creating POD </font>
check whats running:
```
kubectl get all
```
try a trial run and validate YAML:
```
kubectl create -f 01_nginx.pod.yaml --dry-run='client'  --validate='true'
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

then issue the command again:
```
kubectl create -f 01_nginx.pod.yaml
```
Notice: an error will occur as the POD already exisits.

use apply command:
```
kubectl apply -f 01_nginx.pod.yaml
```
Notice: Declarative as state is applied to resource.  If doesnt exist then created.

can use edit / patch for non-disruptive changes
```
kubectl edit -f 01_nginx.pod.yaml -o yaml
```

```
kubectl describe pod nginx
```

```
kubectl exec nginx -it sh
```

---

#### <font color='red'> 2.2.1 Creating POD </font>
to delete the POD:
```
kubectl delete pod nginx
```
can also use:
```
kubectl delete -f 01_nginx.pod.yaml
```
or
kubectl delete -f 01_nginx.pod.yaml
---