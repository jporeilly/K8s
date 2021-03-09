## <font color='red'> 3.2 Deployments </font>
A Deployment provides declarative updates for Pods and ReplicaSets.  

You describe a desired state in a Deployment, and the Deployment Controller changes the actual state to the desired state at a controlled rate. You can define Deployments to create new ReplicaSets, or to remove existing Deployments and adopt all their resources with new Deployments.

In this lab we're going to cover:
* Updating
* Rollback
* Scaling
* Pause & Resume

---

#### <font color='red'> 3.2.1 Deployments </font>
check whats running:
```
kubectl get all
```
deploy nginx 1.7.9:
```
kubectl create -f 01_nginx-deployment.yaml --save-config
```
check deployment:
```
kubectl get deployment nginx-deployment
```
check deployment status:
```
kubectl rollout status deployment/nginx-deployment
```
check replicasets:
```
kubectl get rs
```
view the POD labels:
```
kubectl get pods --show-labels
```
Note: hash label is added by the controller.

---

#### <font color='red'> 3.2.2 Rolling Update </font>
A Deployment's rollout is triggered if and only if the Deployment's Pod template (that is: .spec.template) is changed, 
for example if the labels or container images of the template are updated.  

look at the rollout history:
```
kubectl rollout history deployment
```
lets rollout a new version 1.15.0:
``` 
kubectl apply -f 02_nginx-deployment-rolling.yaml
``` 
view rollout status:
```
kubectl rollout status deployment/nginx-deployment
```
get details of updated deployment:
```
kubectl get deployments
```
check replicaset:
```
kubectl get rs
```
check PODs:
```
kubectl get pods
```
Note: Deployment controller stops deployment and stops scaling up new replicasets.
* Deployment ensures that 80% of the PODs are up and running.
* Deployment ensures that only a certain number of PODs are created above the desired number: 40% max surge

take a look at events:
```
kubectl describe deployments nginx-deployment
```

---

#### <font color='red'> 3.2.3 Rollback Deployment </font>
*  typo while updating the Deployment

lets rollout a new version 1.161 (typo):
``` 
kubectl apply -f 03_nginx-deployment-rolling-update.yaml
``` 
rollout gets stuck:
```
kubectl rollout status deployment/nginx-deployment
```
stop watch on rollout:
```
Ctrl-C
```
get replicaset info:
```
kubectl get rs
```
Note:
get PODs:
```
kubectl get pods
```
Note: deployment controller stops bad rollout.

for further details:
```
kubectl describe deployments
```
check history of rollouts:
```
kubectl rollout history deployment.v1.apps/nginx-deployment
```
Note: You can set .spec.revisionHistoryLimit field in a Deployment to specify how many old ReplicaSets for this Deployment you want to retain. The rest will be garbage-collected in the background. By default, it is 10. Careful as you cant rollback. 

details of each revision:
```
kubectl rollout history deployment.v1.apps/nginx-deployment --revision=2
```
to rollback:
```
kubectl rollout undo deployment.v1.apps/nginx-deployment
```
check if successful:
```
kubectl get deployment nginx-deployment
```
further details:
```
kubectl describe deployment nginx-deployment
```

---

#### <font color='red'> 3.2.3 Scaling Deployments </font>
scale deployment:
```
kubectl scale deployment.v1.apps/nginx-deployment --replicas=10
```
lets use HPA:
```
kubectl autoscale deployment.v1.apps/nginx-deployment --min=10 --max=15 --cpu-percent=80
```
check 10 replicas:
```
kubectl get deploy
```
update with unresolvable tag:
```
kubectl set image deployment.v1.apps/nginx-deployment nginx=nginx:sometag
```
check replicasets:
```
kubectl get rs
```
check how replicas were deployed:
```
kubectl get deploy
```

---

#### <font color='red'> 3.2.3 Pause & Resume Deployments </font>

pause deployment:
```
kubectl rollout pause deployment.v1.apps/nginx-deployment
```
update deployment image:
```
kubectl set image deployment.v1.apps/nginx-deployment nginx=nginx:1.16.1
```
no new rollout:
```
kubectl rollout history deployment.v1.apps/nginx-deployment
```
get rollout status:
```
kubectl get rs
```
make as many updates required:
```
kubectl set resources deployment.v1.apps/nginx-deployment -c=nginx --limits=cpu=200m,memory=512Mi
```
resume deployment:
```
kubectl rollout resume deployment.v1.apps/nginx-deployment
```
watch the status of the rollout:
```
kubectl get rs -w
```
check status:
```
kubectl get rs
```


clean up:
```
kubectl delete -f 01_nginx-deployment.yaml --grace-period=0 --force
kubectl delete -f 02_nginx-deployment-rolling.yaml --grace-period=0 --force
```

---