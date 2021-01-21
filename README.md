# Certifications 

<img src="cert.png">


## POD deletion 

```
❯ kubectl   delete pods --all
pod "ashu-pod-1" deleted
pod "ashupod2" deleted
pod "dhavalpod1" deleted
pod "dhavalpod2" deleted
pod "rajeev-pod-1" deleted
pod "rajeevpod" deleted
pod "rajupod1" deleted
pod "rajupod3" deleted
```

## creation of PODs 

```
❯ kubectl  get po
NAME           READY   STATUS    RESTARTS   AGE
ashu-pod-1     1/1     Running   0          47s
rajupod1       1/1     Running   0          14s
yogesh-pod-1   1/1     Running   0          13s
❯ kubectl  get po ashu-pod-1  -o wide
NAME         READY   STATUS    RESTARTS   AGE   IP               NODE          NOMINATED NODE   READINESS GATES
ashu-pod-1   1/1     Running   0          57s   192.168.27.211   k8s-minion2   <none>           <none>
❯ kubectl  get po ashu-pod-1  --show-labels
NAME         READY   STATUS    RESTARTS   AGE   LABELS
ashu-pod-1   1/1     Running   0          66s   x=helloashu
❯ kubectl  get po   --show-labels
NAME           READY   STATUS              RESTARTS   AGE   LABELS
ashu-pod-1     1/1     Running             0          88s   x=helloashu
rajeev-pod-1   0/1     ContainerCreating   0          2s    x=hellorajeev
rajupod1       1/1     Running             0          55s   x=raju_l1
yogesh-pod-1   1/1     Running             0          54s   x=helloyogesh


```


## listing out all api resources that k8s support

```
❯ kubectl  api-resources
NAME                              SHORTNAMES   APIVERSION                             NAMESPACED   KIND
bindings                                       v1                                     true         Binding
componentstatuses                 cs           v1                                     false        ComponentStatus
configmaps                        cm           v1                                     true         ConfigMap
endpoints                         ep           v1                                     true         Endpoints
events                            ev           v1                                     true         Event
limitranges                       limits       v1                                     true         LimitRange
namespaces                        ns           v1                                     false        Namespace
nodes                             no           v1                                     false        Node
persistentvolumeclaims            pvc          v1                                     true         PersistentVolumeClaim
persistentvolumes                 pv           v1                                     false        PersistentVolume
pods                              po           v1                                     true         Pod
podtemplates                                   v1                      

```


## POD more info 

```
❯ kubectl  describe pod ashu-pod-1
Name:         ashu-pod-1
Namespace:    default
Priority:     0
Node:         k8s-minion2/172.31.81.216
Start Time:   Thu, 21 Jan 2021 10:01:22 +0530
Labels:       x=helloashu
Annotations:  cni.projectcalico.org/podIP: 192.168.27.211/32
              cni.projectcalico.org/podIPs: 192.168.27.211/32
Status:       Running
IP:           192.168.27.211
IPs:
  IP:  192.168.27.211
Containers:
  ashuc1:
    Container ID:   docker://9e8dc6546ebb7cab602a1f93d710c908970388ff5c036f0e19aada3330d73acc
    Image:          nginx
    Image ID:       docker-pullable://nginx@sha256:10b8cc432d56da8b61b070f4c7d2543a9ed17c2b23010b43af434fd40e2ca4aa
    Port:           80/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Thu, 21 Jan 2021 10:01:24 +0530
      
```

## service type in k8s

<img src="stype.png">

## Nodeport 

<img src="nodeport.png">

## nodeport creation 

```
❯ kubectl  create  service  nodeport  ashusvc1  --tcp  1234:80  --dry-run=client -o yaml
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: ashusvc1
  name: ashusvc1
spec:
  ports:
  - name: 1234-80
    port: 1234
    protocol: TCP
    targetPort: 80
  selector:
    app: ashusvc1
  type: NodePort
status:
  loadBalancer: {}
❯ kubectl  create  service  nodeport  ashusvc1  --tcp  1234:80  --dry-run=client -o yaml  >ashusvc1.yaml

```

## service deployment 

```
❯ kubectl apply -f ashusvc1.yaml
service/ashusvc1 created
❯ kubectl  get  service
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
ashusvc1     NodePort    10.109.119.122   <none>        1234:32376/TCP   10s
kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP          19h
❯ kubectl  get  service
NAME            TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
ashusvc1        NodePort    10.109.119.122   <none>        1234:32376/TCP   47s
dhpsvc1         NodePort    10.111.174.162   <none>        4567:32662/TCP   2s
kubernetes      ClusterIP   10.96.0.1        <none>        443/TCP          19h
rajeevc1        NodePort    10.102.64.6      <none>        1235:30974/TCP   21s
santhoshisvc1   NodePort    10.110.169.29    <none>        1261:30971/TCP   22s
seshasvc1       NodePort    10.101.177.128   <none>        2345:31682/TCP   6s
yogeshsvc1      NodePort    10.104.99.50     <none>        9876:31330/TCP   32s

```

## service ip 

```
❯ kubectl get svc -o wide
NAME            TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE   SELECTOR
ashusvc1        NodePort    10.109.119.122   <none>        1234:32376/TCP   22m   x=helloashu
dhpsvc1         NodePort    10.111.174.162   <none>        4567:32662/TCP   22m   x=helloadhp
kubernetes      ClusterIP   10.96.0.1        <none>        443/TCP          20h   <none>
rajeevc1        NodePort    10.102.64.6      <none>        1235:30974/TCP   22m   x=hellorajeev
raju-svc1       NodePort    10.106.203.183   <none>        1234:30007/TCP   19m   x=raju_l1
santhoshisvc1   NodePort    10.110.169.29    <none>        1261:30971/TCP   22m   x=hellosanthoshi
satsvc1         NodePort    10.98.115.45     <none>        1234:32697/TCP   21m   x=satlab1


```



