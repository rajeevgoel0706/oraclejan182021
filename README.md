# testing and checking 

<img src="test.png">

## physical server problems 

<img src="ph.png">


## Bare metal VS. VMS 

<img src="vms.png">

## COntainer intro 

<img src="creintro.png">


## CRE 
<img src="cre.png">


## COntainer support 

<img src="cresup.png">

## Docker products 

<img src="dockerpro.png">


## Docker Desktop explain 

<img src="dd.png">


## Docker Desktop links 

### windows 10 

[w10] ('https://hub.docker.com/editions/community/docker-ce-desktop-windows/')

### mac 

[osx] ('https://hub.docker.com/editions/community/docker-ce-desktop-mac/')

## Docker desktop youtube link for windows 10 

[youtube] ('https://www.youtube.com/watch?v=LLXNjtrd4Zk&t=1s')

# Docker installation on LInux VM 

```
 yum  install docker -y
 
 ```
 
 ## starting docker 
 
 ```
 [root@ip-172-31-81-232 ~]# systemctl  start   docker 
[root@ip-172-31-81-232 ~]# systemctl  enable    docker 
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
[root@ip-172-31-81-232 ~]# systemctl  status    docker 
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2021-01-18 06:34:22 UTC; 22s ago
     Docs: https://docs.docker.com
 Main PID: 3798 (dockerd)
   CGroup: /system.slice/docker.service
           └─3798 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock --default-ulimit nofile=1024...

Jan 18 06:34:21 ip-172-31-81-232.ec2.internal dockerd[3798]: time="2021-01-18T06:34:21.294633656Z" level=info msg=...rpc
Jan 18 06:34:21 ip-172-31-81-232.ec2.internal dockerd[3798]: time="2021-01-18T06:34:21.294658871Z" level=info msg=...rpc
Jan 18 06:34:21 ip-172-31-81-232.ec2.internal dockerd[3798]: time="2021-01-18T06:34:21.294675155Z" level=info msg=...rpc
Jan 18 06:34:21 ip-172-31-81-232.ec2.internal dockerd[3798]: time="2021-01-18T06:34:21.355253784Z" level=info msg=...t."
Jan 18 06:34:21 ip-172-31-81-232.ec2.internal dockerd[3798]: 

```


## docker installation check 

```
[ec2-user@ip-172-31-81-232 ~]$ docker  version 
Client:
 Version:           19.03.13-ce
 API version:       1.40
 Go version:        go1.13.15
 Git commit:        4484c46
 Built:             Mon Oct 12 18:51:20 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server:
 Engine:
  Version:          19.03.13-ce
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       4484c46
  Built:            Mon Oct 12 18:51:50 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.1
  GitCommit:        c623d1b36f09f8ef6536a057bd658b3aa8632828
 runc:
  Version:          1.0.0-rc92
  GitCommit:        ff819c7e9184c13b7c2607fe6c30ae19
  
  ```
  
  
  ## Image registry Docker hub 
  
  ===
  
  [https://hub.docker.com/search?q=java&type=image]
  
  
  
  ## small arch
  
  [docker hub]
  
  <img src="darch.png">
  
  ## checking and searchimg image from docker client 
  
  ```
    8  docker   images  
    9  docker   search  java 
   10  docker   search  python 
   
   ```
   

## Docker image pull 

```
   16  docker pull oraclelinux:8.3  
   17  docker  images
   18  docker  pull alpine 
   19  docker  pull busybox 
   20  docker  images
   21  history 
[ec2-user@ip-172-31-81-232 ~]$ docker  images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
alpine              latest              7731472c3f2a        3 days ago          5.61MB
oraclelinux         8.3                 f4a1f2c861ca        3 days ago          429MB
busybox             latest              b97242f89c8a        4 days ago          1.23MB
python              latest              da24d18bf4bf        5 days ago          885MB
mysql               latest              d4c3cafb11d5        5 days ago          545MB
dockette/jdk8       latest              13041d54229c        2 years ago         164MB
java                latest              d23bdf5b1b1b        4 years ago         643MB

```

## image storage  

```
Total Memory: 3.794GiB
 Name: ip-172-31-81-232.ec2.internal
 ID: MMCW:XVOE:KQO5:T4YA:DMU5:AAFD:ATW7:W3AJ:HN7H:ZCOU:P5QZ:VCOU
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false

[ec2-user@ip-172-31-81-232 ~]$ sudo -i
[root@ip-172-31-81-232 ~]# cd  /var/lib/docker 
[root@ip-172-31-81-232 docker]# ls
builder  buildkit  containers  image  network  overlay2  plugins  runtimes  swarm  tmp  trust  volumes

```


## first container 

```
[ec2-user@ip-172-31-81-232 ~]$ docker  run  --name  ashuc1   alpine:latest   ping google.com 
PING google.com (172.217.12.238): 56 data bytes
64 bytes from 172.217.12.238: seq=0 ttl=109 time=0.829 ms
64 bytes from 172.217.12.238: seq=1 ttl=109 time=0.814 ms
64 bytes from 172.217.12.238: seq=2 ttl=109 time=0.834 ms
64 bytes from 172.217.12.238: seq=3 ttl=109 time=0.823 ms
64 bytes from 172.217.12.238: seq=4 ttl=109 time=0.850 ms
64 bytes from 172.217.12.238: seq=5 ttl=109 time=0.858 ms
64 bytes from 172.217.12.238: seq=6 ttl=109 time=0.821 ms
64 bytes from 172.217.12.238: seq=7 ttl=109 time=0.842 ms
64 bytes from 172.217.12.238: seq=8 ttl=109 time=0.829 ms
64 bytes from 172.217.12.238: seq=9 ttl=109 time=0.820 ms
64 bytes from 172.217.12.238: seq=10 ttl=109 time=0.853 ms
64 bytes from 172.217.12.238: seq=11 ttl=109 time=0.838 ms
64 bytes from 172.217.12.238: seq=12 ttl=109 time=0.802 ms
64 bytes from 172.217.12.238: seq=13 ttl=109 time=0.850 ms
64 bytes from 172.217.12.238: seq=14 ttl=109 time=0.832 ms
64 bytes from 172.217.12.238: seq=15 ttl=109 time=0.837 ms
64 bytes from 172.217.12.238: seq=16 ttl=109 time=0.815 ms
^C
--- google.com ping statistics ---
17 packets transmitted, 17 packets received, 0% packet loss

```

## create a container with parent process in background 

```
[ec2-user@ip-172-31-81-232 ~]$ docker  run  --name  ashuc2  -d    alpine:latest   ping google.com 
cf6315085042bbbc1b39e4b3e04f5baf93af77839f139cc7a6a203ed225dc3d5
[ec2-user@ip-172-31-81-232 ~]$ 
[ec2-user@ip-172-31-81-232 ~]$ 
[ec2-user@ip-172-31-81-232 ~]$ docker   ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
cf6315085042        alpine:latest       "ping google.com"   9 seconds ago       Up 7 seconds                            ashuc2

```

## checking outtput of parent process

```
32  docker  run  --name  ashuc2  -d    alpine:latest   ping google.com 
   33  docker   ps
   34  docker  ps
   35  docker  logs   ashuc2  
   36  docker  logs -f    ashuc2 
   
   ```
   
   ## more docker instructions
   
   ```
   [ec2-user@ip-172-31-81-232 ~]$ docker  ps
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
087147e80d27        alpine:latest       "ping google.com"   44 seconds ago       Up 43 seconds                           santhoshic2
343104d68174        alpine              "ping google.com"   About a minute ago   Up About a minute                       sumeet
cf6315085042        alpine:latest       "ping google.com"   3 minutes ago        Up 3 minutes                            ashuc2
[ec2-user@ip-172-31-81-232 ~]$ 
[ec2-user@ip-172-31-81-232 ~]$ 
[ec2-user@ip-172-31-81-232 ~]$ docker  stop  ashuc2  
ashuc2
[ec2-user@ip-172-31-81-232 ~]$ docker  ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
087147e80d27        alpine:latest       "ping google.com"   2 minutes ago       Up 2 minutes                            santhoshic2
343104d68174        alpine              "ping google.com"   2 minutes ago       Up 2 minutes                            sumeet
[ec2-user@ip-172-31-81-232 ~]$ docker  start  ashuc2 
ashuc2
[ec2-user@ip-172-31-81-232 ~]$ docker  ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
087147e80d27        alpine:latest       "ping google.com"   2 minutes ago       Up 2 minutes                            santhoshic2
343104d68174        alpine              "ping google.com"   2 minutes ago       Up 2 minutes                            sumeet
cf6315085042        alpine:latest       "ping google.com"   4 minutes ago       Up 2 seconds                            ashuc2
[ec2-user@ip-172-31-81-232 ~]$ 
[ec2-user@ip-172-31-81-232 ~]$ 
[ec2-user@ip-172-31-81-232 ~]$ docker  kill  ashuc2 
ashuc2
[ec2-user@ip-172-31-81-232 ~]$ docker  ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
087147e80d27        alpine:latest       "ping google.com"   3 minutes ago       Up 3 minutes                            santhoshic2
343104d68174        alpine              "ping google.com"   4 minutes ago       Up 3 minutes                            sumeet
2570f1d2a4d8        alpine:latest       "ping google.com"   6 minutes ago       Up 1 second                             rajualpine
fca6ffc4d98c        alpine:latest       "ping google.com"   7 minutes ago       Up 10 seconds                           rajeevc2
[ec2-user@ip-172-31-81-232 ~]$ docker  start ashuc2
ashuc2
[ec2-user@ip-172-31-81-232 ~]$ docker  ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
087147e80d27        alpine:latest       "ping google.com"   3 minutes ago       Up 3 minutes                            santhoshic2
343104d68174        alpine              "ping google.com"   4 minutes ago       Up 4 minutes                            sumeet
cf6315085042        alpine:latest       "ping google.com"   6 minutes ago       Up 2 seconds                            ashuc2
2570f1d2a4d8        alpine:latest       "ping google.com"   6 minutes ago       Up 10 seconds                           rajualpine
fca6ffc4d98c        alpine:latest       "ping google.com"   7 minutes ago       Up 19 seconds                           rajeevc2
[ec2-user@ip-172-31-81-232 ~]$ 


```


## kill and remove container 

```
[ec2-user@ip-172-31-81-232 ~]$ docker kill  $(docker  ps  -q)
ef20960e1b5b
087147e80d27
343104d68174
cf6315085042
[ec2-user@ip-172-31-81-232 ~]$ docker  ps 
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
2570f1d2a4d8        alpine:latest       "ping google.com"   9 minutes ago       Up 1 second                             rajualpine
[ec2-user@ip-172-31-81-232 ~]$ docker rm   $(docker  ps  -qa)
ef20960e1b5b
3585085d3adb
087147e80d27
343104d68174
cf6315085042
d567c9d42129
9a3cb54df6eb
fca6ffc4d98c
58a4c9877b8b
3b2538e0e2ce
Error response from daemon: You cannot remove a running container 2570f1d2a4d88195cf6a92f6a9e697f057786d197d108b13b69167221808d398. Stop the container before attempting removal or force remove

```

## docker summary 

<img src="sum.png">

