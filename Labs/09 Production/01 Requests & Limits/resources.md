## <font color='red'> 11.1 Resources </font>
When you specify a Pod, you can optionally specify how much of each resource a Container needs. The most common resources to specify are CPU and memory (RAM); there are others.

When you specify the resource request for Containers in a Pod, the scheduler uses this information to decide which node to place the Pod on. When you specify a resource limit for a Container, the kubelet enforces those limits so that the running container is not allowed to use more of that resource than the limit you set. The kubelet also reserves at least the request amount of that system resource specifically for that container to use 

In this lab we're going to:
* set resource quota for namespace
* deploy Pods and monitor cpu utilization

---

#### <font color='red'>IMPORTANT:</font> 
<strong>Please ensure you start with a clean environment. 
If you have previously run minikube, you will need to delete the existing instance.</strong>

to stop  minikube:
```
minikube stop
```
to delete  minikube:
```
minikube delete
```
start minikube:
```
minikube start
```
enable metrics-server:
```
minikube addons enable metrics-server
```
start tunnel:
```
minikube tunnel
```

--- 

#### <font color='red'> 11.1.1 Pod Resources - Namespace </font>

create a quota-test namespace:
```
kubectl create namespace quota-test
```
deploy cpu-quota:
```
kubectl create -f 01_cpu-quota.yaml -n quota-test --save-config
```
Note: for cpu request=100m cpu with a limit=200m

verify quota was applied:
```
kubectl describe resourcequota/test-cpu-quota --namespace quota-test
```
or 
```
kubectl get quota -n quota-test
```

to test deploy 3 Pods:

deploy Pod-A
```
kubectl create -f 02_Pod-A.yaml -n quota-test --save-config
```
Note: Pod-A cpu request=50m  limit=100m. So 50% cpu has been allocated to Pod-A.

verify quota was applied:
```
kubectl describe resourcequota/test-cpu-quota --namespace quota-test
```
Note: cpu 50% utilized.


deploy Pod-B
```
kubectl create -f 03_Pod-B.yaml -n quota-test --save-config
```
Note: Pod-A 50% cpu Pod-B 50%.

verify quota was applied:
```
kubectl describe resourcequota/test-cpu-quota --namespace quota-test
```
Note: reached limit for namespace.


deploy Pod-C
```
kubectl create -f 04_Pod-C.yaml -n quota-test --save-config
```
Note: Pod-A 50% cpu Pod-B 50% Pod-C 10%

verify quota was applied:
```
kubectl describe resourcequota/test-cpu-quota --namespace quota-test
```
Note: forbidden as exceeded quota.


cleanup:
```
kubectl delete -f 01_cpu-quota.yaml
kubectl delete -f 02_Pod-A.yaml
kubectl delete -f 03_Pod-B.yaml
kubectl delete -f 04_Pod-C.yaml
```

---

#### <font color='red'> 11.1.2 HPA Horizontal Pod Autoscaler </font>
You can use the Kubernetes Horizontal Pod Autoscaler to automatically scale the number of pods in a deployment, replication controller, replica set, or stateful set, based on that resource's CPU or memory utilization, or on other metrics. The Horizontal Pod Autoscaler can help applications scale out to meet increased demand, or scale in when resources are no longer needed. You can set a target metric percentage for the Horizontal Pod Autoscaler to meet when scaling applications. 


Pre-requisites:
* confirm that the Kubernetes Metrics Server


verify KMS:
```
kubectl -n kube-system get deployment/metrics-server
```


deploy apache app:
```
kubectl apply -f https://k8s.io/examples/application/php-apache.yaml
```
deploy HPA:
```
kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10
```
Note replicaset min=1 max=10  cpu=50%

verify HPA:
```
kubectl get hpa
```
run a container with a busybox image to create a load for the Apache web server:
```
kubectl run -it --rm load-generator --image=busybox /bin/sh --generator=run-pod/v1
```
generate a load for the Apache web server
```
while true; do wget -q -O- http://php-apache; done
```
in anew terminal, view HPA:
```
kubectl get hpa
```
after a few mins, view again:
```
kubectl get hpa
```
Note: Horizontal Pod Autoscaler has resized the deployment to 5 replicas, and the utilization target of 50% has been achieved.

verify its been scaled out:
```
kubectl get deployment php-apache
```
terminate load:
```
Ctl + C
```
exit shell:
```
exit
```
in a few mins, check HPA has scaled back:
```
kubectl get hpa
```
verify the deployment has been scaled down:
```
kubectl get deployment php-apache
```

clean up:
```
kubectl delete horizontalpodautoscaler.autoscaling/php-apache
kubectl delete deployment.apps/php-apache service/php-apache
```

---

#### <font color='red'> 11.1.3 VPA Vertical Pod Autoscaler </font>
Vertical Pod autoscaling frees you from having to think about what values to specify for a container's CPU requests and limits and memory requests and limits. The autoscaler can recommend values for CPU and memory requests and limits, or it can automatically update the values.


Pre-requisites:
* confirm that the Kubernetes Metrics Server
* download VPA

verify KMS:
```
kubectl -n kube-system get deployment/metrics-server
```
download VPA:
```
sudo git clone -b vpa-release-0.8 https://github.com/kubernetes/autoscaler.git
```
change to VPA director:
```
cd autoscaler/vertical-pod-autoscaler
```
deploy VPA:
```
./hack/vpa-up.sh
```
verify VPA:
```
kubectl get pods -n kube-system
```


deploy hamster app:
```
kubectl apply -f examples/hamster.yaml
```
verify hamster app:
```
kubectl get pods -l app=hamster
```
check resource Pod quota:
```
kubectl describe pod hamster-xxxxxxxx-xxxxxx
```
watch the hamster Pod:
```
kubectl get --watch pods -l app=hamster
```
when you see new Pod created:
```
kubectl describe pod hamster-xxxxxxxx-xxxxx
```
view the recomendations:
```
kubectl describe vpa/hamster-vpa
```

clean up:
```
kubectl delete -f examples/hamster.yaml
./hack/vpa-down.sh
```

---