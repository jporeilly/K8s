## <font color='red'> 1.2 K8s Cluster Commands</font>
By default, kubectl gets configured to access the kubernetes cluster control plane inside minikube when the minikube start command is executed.  

In this lab we're going to:
* common kubectl commands
* alias commands

---

kubectl version:
```
kubectl version
```
list kubectl commands:
```
kubectl
```
view cluster info:
```
kubectl cluster-info
```
check nodes:
```
kubectl get nodes -o wide
```
check everything (PODs & Services):
```
kubectl get all
```
check just PODs:
```
kubectl get pods
```
simple way to deploy a POD:
```
kubectl run [container-name] --image=[image-name]
```
port forward to allow external access:
```
kubectl port-forward [POD] [ports]
```
expose a port for deployment:
```
kubectl expose
```
create a resource:
```
kubectl create [resource]
```
create or modify resource:
```
kubectl apply [resource]
```

For kubectl commands: https://kubernetes.io/docs/reference/kubectl/cheatsheet/

Document: <a href="./K8s Commands.pdf" target="_top">K8s Commands</a>

---

**Tip:**
Alias the commands:

For example: alias ksysgdepwslowidel='kubectl --namespace=kube-system get deployment --watch --show-labels -o=wide -l'

Try some of the common commands:
```
k='kubectl'
kg='kubectl get'
kgpo='kubectl get pod'
```  

Syntax explanation:
```
k=kubectl
sys=--namespace kube-system
```
commands:
```
g=get
d=describe
rm=delete
a:apply -f
ak:apply -k
k:kustomize
ex: exec -i -t
lo: logs -f
```
resources:
```
po=pod
dep=deployment
ing=ingress
svc=service
cm=configmap
sec=secret
ns=namespace
no=node
```
flags:
```
output format: oyaml, ojson, owide
all: --all or --all-namespaces depending on the command
sl: --show-labels
w=-w/--watch
value flags (should be at the end):
n=-n/--namespace
f=-f/--filename
l=-l/--selector 
```

> further details: https://github.com/ahmetb/kubectl-aliases

---