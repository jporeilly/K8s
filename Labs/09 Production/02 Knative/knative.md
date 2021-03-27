## <font color='red'> 11.2 Knative </font>
Knative is an open source community project that adds components to Kubernetes for deploying, running, and managing serverless, cloud-native applications. It enables more productive development with less interaction with Kubernetes' infrastructure.

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

Pre-requistites for knative
* Knative CLI
* Go v1.14+
* install Knative
* install Kourier
* configure DNS

<font color='red'>The pre-requisites have already been installed</font>


**install Knative CLI:**

create knative directory:
```
mkdir knative
```
change to directory:
```
cd knative/
```
clone from knative github:
```
sudo git clone https://github.com/knative/client.git
```
change to client directory:
```
cd client/
```
build knative:
```
hack/build.sh -f
```
copy to local:
```
sudo cp kn /usr/local/bin
```
check version:
```
kn version
```

Version:        v20201018-local-40a84036
Build Date:   2020-10-18 20:00:37
Git Revision: 40a84036
Supported APIs:
* Serving
  - serving.knative.dev/v1 (knative-serving v0.18.0)
* Eventing
  - sources.knative.dev/v1alpha2 (knative-eventing v0.18.0)
  - eventing.knative.dev/v1beta1 (knative-eventing v0.18.0)


**install Knative:**

install CRDs:
```
kubectl apply --filename https://github.com/knative/serving/releases/download/v0.21.0/serving-crds.yaml
```
install core components:
```
kubectl apply --filename https://github.com/knative/serving/releases/download/v0.21.0/serving-core.yaml
```
verify deployment:
```
kubectl wait deployment --all --timeout=-1s --for=condition=Available -n knative-serving
```


**install kourier**

install Knative Kourier controller:
```
kubectl apply --filename https://github.com/knative/net-kourier/releases/download/v0.21.0/kourier.yaml
```
verify deployment:
```
kubectl get deployments -n kourier-system
```
configure Knative serving to use Kourier by default:
```
kubectl patch configmap/config-network \
  --namespace knative-serving \
  --type merge \
  --patch '{"data":{"ingress.class":"kourier.ingress.networking.knative.dev"}}'
```


**configure DNS**

fetch the External IP or CNAME:
```
kubectl --namespace kourier-system get service kourier
```
patch the configmap with the ClusterIP address:
```
kubectl patch configmap -n knative-serving config-domain -p "{\"data\": {\"[ClusterIP].nip.io\": \"\"}}"
```

---

#### <font color='red'> 11.1.1 Deploy Knative Service </font>

to view kn commands:
```
kn
```
deploy "hello-world" app:
```
kn service create hello \
--image gcr.io/knative-samples/helloworld-go \
--port 8080 \
--env TARGET=Knative
```
or 
you can deploy to another namespace:
```
kubectl create -f 01_kn-service.yaml --save-config
```
verify deployment:
```
 > check in browser: http://hello.hello.[ClusterIP].nip.io
```
Note: the URL is in the deployment output

describe the service:
```
kn service describe hello
```
describe and list the routes:
```
kn route describe hello
```

---


#### <font color='red'> 11.1.2 Deploy Knative App </font>

deploy "hello world" app
```
kubectl create -f 02_my-app.yaml  --save-config
```
curl into app:
```
curl  http://helloworld.default.[ClusterIP].nip.io
```

 > check in browser: http://helloworld.default.[ClusterIP].nip.io

---


clean up:
```
kn service delete hello
```

---