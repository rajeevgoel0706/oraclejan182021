# Docker client options

<img src="cli.png">

# Docker compose 

## Cleaning up docker engine 

```
 496  docker  rm  $(docker ps -qa) -f
  497  docker  images
  498  docker rmi $(docker images -q) -f
  499  docker  images
  500  docker  network  rm  $(docker network ls -q)
  
```

## to check compose file version history 

[history] ('https://docs.docker.com/compose/compose-file/')


## Example 1

```
[ec2-user@ip-172-31-81-232 ashucompose1]$ cat  docker-compose.yml 
version: "3.8"  # compose file version 
services:
 ashuapp1:  # to define info about containers 
  image: alpine
  container_name: ashuc1
  command: ping google.com
  
```

## Deploy using compose 

```
[ec2-user@ip-172-31-81-232 ashucompose1]$ docker-compose up -d 
Creating network "ashucompose1_default" with the default driver
Pulling ashuapp1 (alpine:)...
latest: Pulling from library/alpine
596ba82af5aa: Pull complete
Digest: sha256:d9a7354e3845ea8466bb00b22224d9116b183e594527fb5b6c3d30bc01a20378
Status: Downloaded newer image for alpine:latest
Creating ashuc1 ... done
[ec2-user@ip-172-31-81-232 ashucompose1]$ docker-compose ps
 Name        Command       State   Ports
----------------------------------------
ashuc1   ping google.com   Up       

```


## clean up 

```
[ec2-user@ip-172-31-81-232 ashucompose1]$ docker-compose down
Stopping ashuc1 ... done
Removing ashuc1 ... done
Removing network ashucompose1_default

```


## Example 2

```
[ec2-user@ip-172-31-81-232 ashucompose1]$ cat  jsp-compose.yaml 
version: "3.8"
services:
 ashujavaapp1:
  image: ashujsp:v1
  container_name: ashuwb1
  ports:  # is an array object 
   - "2233:8080"
   
```


## Deployment 

```
[ec2-user@ip-172-31-81-232 ashucompose1]$ docker-compose  -f  jsp-compose.yaml    up  -d
Pulling ashujavaapp1 (dockerashu/ashujsp:v1)...
v1: Pulling from dockerashu/ashujsp
b9a857cbf04d: Pull complete
d557ee20540b: Pull complete
3b9ca4f00c2e: Pull complete
667fd949ed93: Pull complete
661d3b55f657: Pull complete
511ef4338a0b: Pull complete
a56db448fefe: Pull complete
00612a99c7dc: Pull complete
326f9601c512: Pull complete
c547db74f1e1: Pull complete
fefc2a019ee0: Pull complete
19b347937c5c: Pull complete
c532444d9622: Pull complete
Digest: sha256:727d8fb7b0895a1c092c840e97c731ee72d35ad366d199b64a47bb3741ad1c3e
Status: Downloaded newer image for dockerashu/ashujsp:v1
Creating ashuwb1 ... done
[ec2-user@ip-172-31-81-232 ashucompose1]$ docker-compose  -f  jsp-compose.yaml    ps
 Name         Command       State           Ports         
----------------------------------------------------------
ashuwb1   catalina.sh run   Up      0.0.0.0:2233->8080/tcp
[ec2-user@ip-172-31-81-232 ashucompose1]$ !cur
curl https://ipinfo.io/json 
{
  "ip": "54.161.225.68",
  "hostname": "ec2-54-161-225-68.compute-1.amazonaws.com",
  "city": "Virginia Beach",
  "region": "Virginia",
  "country": "US",
  "loc": "36.7957,-76.0126",
  "org": "AS14618 Amazon.com, Inc.",
  "postal": "23479",
  "timezone": "America/New_York",
  "readme": "https://ipinfo.io/missingauth"


```


