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

# VOlumes in k8s

## EmptyDir volume 

```
kubectl  run  ashupod1  --image=alpine  --namespace ashu-space --dry-run=client -o yaml  >ashuemppod.yml

```

## yaml of EmptyDir type 

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: ashupod1
  name: ashupod1
  namespace: ashu-space
spec:
  nodeName: k8s-minion2 # scheduling pod in minion 2 
  volumes: # creation of volumes 
  - name: ashuvol1 # name of volume 
    emptyDir: {} # type of Volume 
  containers:
  - image: alpine
    name: ashupod1
    volumeMounts:  # mount volume that we created above 
    - name: ashuvol1  # name of volume
      mountPath: /mnt/data  # path inside container 
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

```

##  access the pod 

```
❯ kubectl exec -it ashupod1  -n ashu-space  -- sh
/ # ps  -e
PID   USER     TIME  COMMAND
    1 root      0:00 /bin/sh -c while true;do date >>/mnt/data/time.txt; sleep 3;done
   21 root      0:00 sleep 3
   22 root      0:00 sh
   28 root      0:00 ps -e
/ # cd   /mnt/data/
/mnt/data # ls
time.txt
/mnt/data # cat  time.txt 
Fri Jan 22 06:30:32 UTC 2021
Fri Jan 22 06:30:35 UTC 2021
Fri Jan 22 06:30:38 UTC 2021
Fri Jan 22 06:30:41 UTC 2021

```

## Pod with multiple container 

<img src="multicpod.png">

## multi Pod container 

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: ashupod1
  name: ashupod1
  namespace: ashu-space
spec:
  nodeName: k8s-minion2 # scheduling pod in minion 2 
  volumes: # creation of volumes 
  - name: ashuvol1 # name of volume 
    emptyDir: {} # type of Volume 
  containers:
  - image: nginx 
    name: ashuc2
    ports:
    - containerPort: 80
    volumeMounts:
    - name: ashuvol1
      mountPath: /usr/share/nginx/html/ # app hosting location 
  - image: alpine   #  helper container 
    name: ashupod1  # name of container for data generation purpose 
    command: ["/bin/sh","-c","while true;do date >>/mnt/data/index.html; sleep 3;done"] # parent process of container 
    volumeMounts:  # mount volume that we created above 
    - name: ashuvol1  # name of volume
      mountPath: /mnt/data  # path inside container 
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

```

# HostPath 

## Example Portainer 

```
kubectl  create deployment  ashuwebui --image=portainer/portainer --namespace ashu-space  --dry-run=client -o yaml >portainer.yaml

```


## portainer yaml 

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: ashuwebui
  name: ashuwebui
  namespace: ashu-space
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ashuwebui
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: ashuwebui
    spec:
      volumes:
      - name: ashuvolx1 # name of volume
        hostPath:
         path: /var/run/docker.sock # minion Node Mount Source 
         type: Socket
      containers:
      - image: portainer/portainer
        name: portainer
        ports:
        - containerPort: 9000
        volumeMounts:
        - name: ashuvolx1
          mountPath: /var/run/docker.sock # portainer mount path
        resources: {}
status: {}

```

# volume link of K8s

['link'] ('https://kubernetes.io/docs/concepts/storage/volumes/')


# KAAS in cloud 

<img src="kaas.png">

## end user based application access model 

<img src="app.png">

## multi context in k8s

```
4294  aws eks describe-cluster  --name oraclek8s1
 4295  aws configure
 4296  aws eks describe-cluster  --name oraclek8s1
 4297  history
 4298  aws eks help  |   grep -i kubeconf
 4299* cat ~/.kube/config 
 4300* kubectl config view
 4301* docker  context  ls
 4302* kubectl  config get-contexts
 4303  aws eks update-kubeconfig  --name oraclek8s1
 4304* kubectl  config get-contexts
 4305* kubectl  get  nodes
 4306* kubectl  config use-contexts kubernetes-admin@kubernetes 
 4307* kubectl  config set-contexts kubernetes-admin@kubernetes 
 4308* kubectl config 
 4309* kubectl  config use-context kubernetes-admin@kubernetes 
 4310* kubectl  config get-contexts
 4311* kubectl  get  nodes
 4312* kubectl  config use-context arn:aws:eks:us-east-1:061112302981:cluster/oraclek8s1
 4313* kubectl  get  nodes
 4314  kubectl  config view
 4315  cat  /Users/fire/.kube/config
 
 ```
 
 
 # Service account information 
 
 ```
 ❯ kubectl  get sa  -n ashu-space
NAME      SECRETS   AGE
default   1         6h16m
❯ kubectl  get secret   -n ashu-space
NAME                  TYPE                                  DATA   AGE
default-token-rk4fn   kubernetes.io/service-account-token   3      6h16m
❯ kubectl  describe secret  default-token-rk4fn  -n ashu-space
Name:         default-token-rk4fn
Namespace:    ashu-space
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: default
              kubernetes.io/service-account.uid: 9a7c7bf6-8211-4343-856c-c0a9c59b3bc8

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1066 bytes
namespace:  10 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IjdROFl2T0IxQTRiSGFCaDVkWEdRcFNoOWg2VEZBLWtXY1pPSVV6emswbUkifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJhc2h1LXNwYWNlIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZWNyZXQubmFtZSI6ImRlZmF1bHQtdG9rZW4tcms0Zm4iLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiZGVmYXVsdCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjlhN2M3YmY2LTgyMTEtNDM0My04NTZjLWMwYTljNTliM2JjOCIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDphc2h1LXNwYWNlOmRlZmF1bHQifQ.K-CSrdV_d7fhAgKuB3HEylPlYtPtplso4dkizeQt_GmQqsu8NIGxu3qtmO5B4zVOodrg4AGFWSm4JFB4m0VaLTxEQeserschPBE9v_34CcSZlh7MIvOn4zJMMK4y9SQXjcp2PVx3HDXfCP9SOcHVdgqWcE6Z_Hnyo9A-Cly2IS8pUq5md0ZupnQEdj2TUuhFSPE0MwbiDSntUz7_7x_J7nkHeMuz3Ihr8vwYu0ZFzZOyBlsVc-aiSmZW7HKUxvJKO59A4QwK3lyrhK4zZJtvt6GvmD9RAJ4mx581uLeFP9TuV1C6rbkax9HxnCICf5cGhu0OzuYfWC2l1zqFKhf4sg

```

## RBAC 

<img src="rbac.png">

## dashboard deployment 

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
namespace/kubernetes-dashboard created
serviceaccount/kubernetes-dashboard created
service/kubernetes-dashboard created
secret/kubernetes-dashboard-certs created
sec

```

## checking dashboard 

```
❯ kubectl  get deploy -n kubernetes-dashboard
NAME                        READY   UP-TO-DATE   AVAILABLE   AGE
dashboard-metrics-scraper   1/1     1            1           39s
kubernetes-dashboard        1/1     1            1           41s
❯ kubectl  get po  -n kubernetes-dashboard
NAME                                         READY   STATUS    RESTARTS   AGE
dashboard-metrics-scraper-7b59f7d4df-m584d   1/1     Running   0          54s
kubernetes-dashboard-74d688b6bc-xkchq        1/1     Running   0          55s
❯ kubectl  get sa   -n kubernetes-dashboard
NAME                   SECRETS   AGE
default                1         71s
kubernetes-dashboard   1         70s
❯ kubectl  get secret   -n kubernetes-dashboard
NAME                               TYPE                                  DATA   AGE
default-token-bt4kg                kubernetes.io/service-account-token   3      80s
kubernetes-dashboard-certs         Opaque                                0      78s
kubernetes-dashboard-csrf          Opaque                                1      77s
kubernetes-dashboard-key-holder    Opaque                                2      76s
kubernetes-dashboard-token-295zk   kubernetes.io/service-account-token   3      79s
❯ kubectl  get svc  -n kubernetes-dashboard
NAME                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
dashboard-metrics-scraper   ClusterIP   10.108.228.85   <none>        8000/TCP   83s
kubernetes-dashboard        ClusterIP   10.105.37.20    <none>        443/TCP    90s

```

## changing service to nodeport 

```
❯ kubectl  edit  svc  kubernetes-dashboard    -n kubernetes-dashboard
service/kubernetes-dashboard edited
❯ kubectl  get svc  -n kubernetes-dashboard
NAME                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)         AGE
dashboard-metrics-scraper   ClusterIP   10.108.228.85   <none>        8000/TCP        3m39s
kubernetes-dashboard        NodePort    10.105.37.20    <none>        443:30225/TCP   3m46s

```



