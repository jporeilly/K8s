## <font color='red'> 2.1 Creating & Accessing PODs </font>

In this lab we're going to:
* Deploy an application
* Run through some common kubectl commands
* Delete POD

check whats running:
```
kubectl get all
```
try a trial run and validate YAML:
```
kubectl create –f 01_nginx.pod.yaml --dry-run --validate=true
```
create nginx deployment:
```
kubectl create –f 01_nginx.pod.yaml --save-config
```
Notice: stores the configuration in annotations  

check the YAML:
```
kubectl get pod nginx -o yaml
```
Notice: annotations  

then issue the command again:
```
kubectl create –f 01_nginx.pod.yaml
```
Notice: an error will occur as the POD already exisits.

use apply command:
```
kubectl apply –f 01_nginx.pod.yaml
```
Notice: Declarative as state is applied to resource.  If doesnt exist then created.

can use edit / patch for non-disruptive changes
```
kubectl edit -f nginx.pod.yml
```

```
kubectl describe pod nginx
```

```
kubectl exec nginx -it sh
```

to delete the POD:
```
kubectl delete pod nginx
```
can also use:
```
kubectl delete 01_nginx.pod.yaml
```


```
Kubectl delete –f nginx.pod.yaml
```


---