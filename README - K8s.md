# Kubernetes (K8s)

Kubernetes, also known as K8s, is an open source system for managing containerized applications
across multiple hosts. 

- See our documentation on [Kubernetes.io].
- For details about who's involved and how Kubernetes plays a role,
read the CNCF [Announcement].

- To use Kubernetes code as a library in other applications, see the [List of published components](https://git.k8s.io/kubernetes/staging/README.md).
Use of the `k8s.io/kubernetes` module or `k8s.io/kubernetes/...` packages as libraries is not supported.


## <font color='red'> Developing K8s </font>
The [community repository] hosts all information about
building Kubernetes from source, how to contribute code
and documentation, who to contact about what, etc.

If you want to build Kubernetes right away there are two options:

##### You have a working [Go environment].
```
mkdir -p $GOPATH/src/k8s.io
cd $GOPATH/src/k8s.io
git clone https://github.com/kubernetes/kubernetes
cd kubernetes
make
```

##### You have a working [Docker environment].
```
git clone https://github.com/kubernetes/kubernetes
cd kubernetes
make quick-release
```

For the full story, head over to the [developer's documentation].

## <font color='red'> Support </font>
If you need support, start with the [troubleshooting guide],
and work your way through the process that we've outlined.

That said, if you have questions, reach out to us
[one way or another][communication].

[Announcement] https://cncf.io/news/announcement/2015/07/new-cloud-native-computing-foundation-drive-alignment-among-container  
[Borg] https://research.google.com/pubs/pub43438.html  
[CNCF] https://www.cncf.io/about  
[Communication] https://git.k8s.io/community/communication  
[Community repository] https://git.k8s.io/community  
[Containerized applications] https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/  
[Developer's documentation] https://git.k8s.io/community/contributors/devel#readme  
[Docker environment] https://docs.docker.com/engine  
[Go environment] https://golang.org/doc/install  
[GoDoc] https://godoc.org/k8s.io/kubernetes  
[GoDoc Widget] https://godoc.org/k8s.io/kubernetes?status.svg  
[Interactive tutorial] https://kubernetes.io/docs/tutorials/kubernetes-basics  
[Kubernetes.io] https://kubernetes.io  
[Scalable Microservices with Kubernetes] https://www.udacity.com/course/scalable-microservices-with-kubernetes--ud615  
[Troubleshooting guide] https://kubernetes.io/docs/tasks/debug-application-cluster/troubleshooting/  
