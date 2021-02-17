## <font color='red'> Multi container PODs </font>

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

zero-trust security model and you'd like to encrypt all traffic on the network.
```
to delete a POD:
```
kubectl delete deployment my-nginx
```
check whats running:
```
kubectl get all
```
> Notice POD is Terminating  

set a Watch:
```
kubectl get pods --watch
```
---