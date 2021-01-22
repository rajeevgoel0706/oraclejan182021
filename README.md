# Container process summary 

<img src="container.png">


# Namespace in k8s

```
❯ kubectl  get  namespace
NAME              STATUS   AGE
default           Active   43h
kube-node-lease   Active   43h
kube-public       Active   43h
kube-system       Active   43h
❯ kubectl  get  ns
NAME              STATUS   AGE
default           Active   43h
kube-node-lease   Active   43h
kube-public       Active   43h
kube-system       Active   43h

```

# kube-system namespace 

```
❯ kubectl  get  po  -n kube-system
NAME                                       READY   STATUS    RESTARTS   AGE
calico-kube-controllers-744cfdf676-c2zpv   1/1     Running   2          43h
calico-node-5mt7m                          1/1     Running   2          43h
calico-node-9nrw6                          1/1     Running   2          43h
calico-node-mctqs                          1/1     Running   2          43h
calico-node-sw9rg                          1/1     Running   2          43h
coredns-74ff55c5b-2gvn7                    1/1     Running   2          43h
coredns-74ff55c5b-tq7pd                    1/1     Running   2          43h
etcd-k8s-master                            1/1     Running   2          43h
kube-apiserver-k8s-master                  1/1     Running   2          43h
kube-controller-manager-k8s-master         1/1     Running   2          43h
kube-proxy-4dkgg                           1/1     Running   2          43h
kube-proxy-h5s4r                           1/1     Running   2          43h
kube-proxy-lk2qb                           1/1     Running   2          43h
kube-proxy-rm4gv                           1/1     Running   2          43h
kube-scheduler-k8s-master                  1/1     Running   2          43h


```

## namespace dependency 

<img src="nsdep.png">

## create custom namespace 

```
❯ kubectl  create  namespace  ashu-space --dry-run=client -o yaml
apiVersion: v1
kind: Namespace
metadata:
  creationTimestamp: null
  name: ashu-space
spec: {}
status: {}
❯ kubectl  create  namespace  ashu-space
namespace/ashu-space created
❯ kubectl  get  ns
NAME              STATUS   AGE
ashu-space        Active   6s
default           Active   43h
kube-node-lease   Active   43h
kube-public       Active   43h
kube-system       Active   43h

```

## Expose deployment to create service 

```
 kubectl  expose deployment  ashu-depweb --type NodePort --port 80 --name mysvc1  -n ashu-space
 
```

###

```
❯ kubectl  get  all  -n ashu-space
NAME                               READY   STATUS    RESTARTS   AGE
pod/ashu-depweb-75869959fb-fvws6   1/1     Running   0          11m

NAME             TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/mysvc1   NodePort   10.104.132.255   <none>        80:30032/TCP   6m48s

NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/ashu-depweb   1/1     1            1           11m

NAME                                     DESIRED   CURRENT   READY   AGE
replicaset.apps/ashu-depweb-75869959fb   1         1         1       11m

```


## Package YAML 

```
# creating name space 
apiVersion: v1
kind: Namespace
metadata:
 name: ashutest1

#  creating POD 

--- 
apiVersion: v1
kind: Pod
metadata:
 name: ashupod123
 Namespace: ashutest1
 labels:
  x: helloashu101
spec:
 containers:
 - image: nginx
   name: ashuc1
   ports:
   - containerPort: 80
# creating service 
---
apiVersion: v1
kind: Service
metadata:
 name: ashusvc111
 Namespace: ashutest1
spec:
 ports:
 - name: ashuport
   port: 1234
   targetPort: 80
   protocol: TCP
 type: NodePort
 Selector:
   x: helloashu101 
   
   
```


