## <font color='red'> 4.1 Services </font>
Now that you have a continuously running, replicated application you can expose it on a network.

Services are an abstract way to expose an application running on a set of Pods as a network service.
With Kubernetes you don't need to modify your application to use an unfamiliar service discovery mechanism. Kubernetes gives Pods their own IP addresses and a single DNS name for a set of Pods, and can load-balance across them.

K8s provides 4 types of Services:
* ClusterIP - exposes IPs internally in the Cluster
* NodePort - exposes IPs externally
* Loadbalancer
* ExternalName

External:
* Ingress
* Egress


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
start tunnel:
```
minikube tunnel
```

--- 

#### <font color='red'> 4.1.1 Services - ClusterIP </font>

check whats running:
```
kubectl get all
```
deploy nginx:
```
kubectl create -f 01_nginx.yaml --save-config
```
check whats running:
```
kubectl get pods -l run=nginx -o wide
```
check Pod IPs:
```
kubectl get pods -l run=nginx -o yaml | grep podIP
```
create service - YAML:
```
kubectl create -f 02_nginx-service-clusterip.yaml --save-config
```
or

create a service (use the YAML manifest):
```
kubectl expose deployment/nginx-clusterip
```
check service:
```
kubectl get svc nginx-clusterip
```
Note: ClusterIP is default.

describe service:
```
kubectl describe svc nginx-clusterip
```
view endpoints:
```
kubectl get ep nginx-clusterip
```
Note: You can try and access the Pod in a browser but no go as these are internal IPs. 

view nginx Pod IP:
```
kubectl get pods -l run=nginx -o wide
```
access the Service with a browser:

> http://[service-ip]

proxy the request:
```
kubectl proxy --port 8080
```
to access the Pods:

 > http://127.0.0.1:8080/api/v1/namespaces/default/pods

to access the Service:

 > http://127.0.0.1:8080/api/v1/namespaces/default/services/nginx-clusterip/

Note: this just connects to the service and is useful for debugging

Note: Map Service IP to localhost in /etc/hosts file..

** You can change the service:port using targetport**

delete existing service:
``` 
kubectl delete -f 02_nginx-service-clusterip.yaml
```
new service with targetport:
```
kubectl apply -f 03_nginx-service-clusterip-targetport.yaml
```
check the service:
```
kubectl get svc nginx-clusterip-targetport
```
access the Service with a browser:

> http://[service-ip]:8080

Note: The application itself is still configured to listen on port 80. Kubernetes Service manages the translation between the two.

or
```
minikube service nginx-clusterip --url
```

clean up:
```
kubectl delete -f 01_nginx.yaml
kubectl delete -f 02_nginx-service-clusterip.yaml # not required if you used clusterip-targetport
kubectl delete -f 03_nginx-service-clusterip-targetport.yaml
```
to remove the proxy:
```
ps -ef | grep "kubectl proxy"
```
note the PID, then kill:
```
kill -9 [pid]
```

---

#### <font color='red'> 4.1.2 Services - NodePort </font>
A NodePort is an open port on every node of your cluster. Kubernetes transparently routes incoming traffic on the NodePort to your service, even if your application is running on a different node.


check whats running:
```
kubectl get all
```
deploy nginx:
```
kubectl create -f 01_nginx.yaml --save-config
```
check whats running:
```
kubectl get pods -l run=nginx -o wide
```
check Pod IPs:
```
kubectl get pods -l run=nginx -o yaml | grep podIP
```
create a service:
```
kubectl create -f 04_nginx-service-nodeport.yaml
```

check service:
```
kubectl get svc nginx-nodeport
```
describe service:
```
kubectl describe svc nginx-nodeport
```
view endpoints:
```
kubectl get ep nginx-nodeport -o yaml
```
check ports:
```
kubectl get all
```
Note: port 80 has been opened
to access the Service:

 > http://[nodeport-ip]


clean up:
```
kubectl delete -f 01_nginx.yaml
kubectl delete -f 04_nginx-service-nodeport.yaml
```

---

#### <font color='red'> 4.1.3 Services - LoadBalancers </font>
When running in the cloud, such as EC2 or Azure, it's possible to configure and assign a Public IP address issued via the cloud provider. This will be issued via a Load Balancer such as ELB. This allows additional public IP addresses to be allocated to a Kubernetes cluster without interacting directly with the cloud provider.


#### <font color='red'>IMPORTANT:</font> 
<strong>Please ensure you start with a clean environment. 
As we are replacing the default minikube loadbalancer, its recommended to start with a clean environment. 

**Do not start the minikube tunnel.**</strong>


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

The lab configures a metalLB in front of minikube K8s to forward requests to an Niginx server, using ClusterIP.


Using metalLB in front of a NodePort

check whats running:
```
kubectl get all
```
create a namespace for LB 
```
kubectl create -f 05_metalLB-namespace.yaml  --save-config
```
check namespace:
```
kubectl get ns
```
deploy metalLB:
```
kubectl create -f 06_metalLB.yaml --save-config
```
create a secret on first install:
```
kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)"
```
check inside namespace:
```
kubectl get all -n metallb-system
```
get minikube ip:
```
minikube ip
```
deploy configmap:
```
kubectl create -f 07_configmap-addresses.yaml --save-config
```
Note: you may need to configure the configmap for a range of addresses.

check configmap:
```
kubectl describe cm config -n metallb-system
```
deploy metalLB service:
```
kubectl create -f 08_metalLB-service-loadbalancer.yaml --save-config
```
check service:
```
kubectl get svc metal-loadbalancer
```
deploy nginx:
```
kubectl create -f 09_nginx.yaml --save-config
```
access nginx server:

 > http://[clusterIP]

 Note: no minikube loadbalancer between client request 10.x.x.x and the NodePort 

start minikube tunnel:
```
minikube tunnel
```
Notice: picks up metalLB-loadbalancer service


clean up:
```
kubectl delete -f 05_metalLB-namespace.yaml
kubectl delete -f 06_metalLB.yaml
kubectl delete -f 07_configmap-addresses.yaml
kubectl delete -f 08_metalLB-service-loadbalancer.yaml
kubectl delete -f 09_nginx.yaml
```

---

#### <font color='red'> 4.1.4 Services - ExternalIP </font>
Maps the Service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a CNAME record with its value. No proxying of any kind is set up.


update the definition to the current cluster's IP address:
```
sed -i 's/HOSTIP/172.17.0.98/g' 06_nginx-externalip.yaml
```
deploy nginx + service:
```
kubectl apply -f 06_nginx-externalip.yaml
```

kubectl get svc


kubectl describe svc/nginx-externalip


kubectl get all


> http://172.17.0.98


clean up:
```
kubectl delete -f 06_nginx-externalip.yaml
```

---

#### <font color='red'> 4.1.5 External Access - Ingress </font>
Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.

check whats running:
```
kubectl get all
```
enable ingress addon:
```
minikube addons enable ingress
```
check pods in kube-system:
```
kubectl get pods --all-namespaces | grep -i nginx-controller
```
deploy ingress-nginx controller:
```
kubectl create -f 11_ingress-nginx-resources.yaml --save-config
```
check ingress-nginx:
```
kubectl get pods -n ingress-nginx -l app.kubernetes.io/name=ingress-nginx
```
deploy httpd app + service:
```
kubectl create -f 12_httpd-deployment.yaml --save-config
```
deploy nginx app + service:
```
kubectl create -f 13_nginx-deployment.yaml --save-config
```
verify status:
```
kubectl get deployments.apps httpd-deployment
kubectl get deployments.apps nginx-deployment
```
verify services:
```
kubectl get service nginx-service httpd-service
```
deploy ingress resource:
```
kubectl create -f 14_web-ingress.yaml --save-config
```
verify status:
```
kubectl get ingress name-based-virtualhost-ingress
```
check HOSTS:
```
kubectl describe ingress name-based-virtualhost-ingress
```
edit /etc/hosts:
```
sudo nano /etc/hosts
```
add the following:
```
add IP address of name-based-virtualhost-ingress
add HOSTS: httpd.example.com, nginx.example.com
```
