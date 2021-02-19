## <font color='red'> Multi container PODs </font>
Pre-req: ensure cert-manager is installed

install cert-manager:
```
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.2.0/cert-manager.yaml
```
verify:
```
kubectl get pods --namespace cert-manager
```
check whats running:
```
kubectl get all
```
deploy elasticsearch:
```
kubectl create -f es-deployment.yml
```
Note: The discovery.type environment variable is necessary to get it running with a single replica.  
      Elasticsearch will listen on port 9200 over HTTP by default

check whats running:
```
kubectl get all
```
> Note: This may take a little time  

---

deploy sleep:
```
kubectl create -f sleep.yml
```
check PODs:
```
kubectl get pods
```
> Notice new sleep POD created

curl from sleep to elasticsearch container:
```
kubectl exec [sleep-xxxx-pod-name] -it -- sh
```
curl:
```
curl  -- curl http://elasticsearch:9200 
```
the output should be something like:
```
{
  "name" : "elasticsearch-77d857c8cf-dsmqf",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "MphboEo4QLCKMHh7OWM8WQ",
  "version" : {
    "number" : "7.9.3",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "c4138e51121ef06a6404866cddc601906fe5c868",
    "build_date" : "2020-10-16T10:36:16.141335Z",
    "build_snapshot" : false,
    "lucene_version" : "8.6.2",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```
---

zero-trust security model and you'd like to encrypt all traffic on the network.

take a look at the slide deck..

** delete existing exlasticsearch POD
```
kubectl delete -f es-deployment.yml
```
check everything:
```
kubectl get all
```

---

add nginx proxy:
```
kubectl create -f es-deployment-secure.yml
```
Note: This will take some time as a new POD with Nginx and Elasticsearch has to be created.
check everything:
```
kubectl get all
```
curl from sleep to elasticsearch container:
```
kubectl exec [sleep-xxxx-pod-name] -it -- sh
```
curl:
```
curl  -- curl -k https://elasticsearch:9200 
```
Note: The -k version is necessary for self-signed TLS certificates. In a production environment, you'd want to use a trusted certificate.

try again with uncrypted request:
```
curl  -- curl http://elasticsearch:9200 
```
check the logs:
