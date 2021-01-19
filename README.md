# Docker Namespaces 

<img src="ns.png">

## Cgroups in containers

```
267  docker run -d --name x1  alpine ping fb.com 
  268  docker  ps 
  269  docker  stats x1 
  270  history 
  271  docker  stats 
  272  history 
  273  docker ps
  274  docker run -d --name x3  --memory 100m   alpine ping fb.com 
  
 ```
 
 ## CPU and ram limiting 
 
 ```
 282  docker run -d --name x4  --cpuset-cpus=0 --cpu-shares=20   alpine ping fb.com 
  283  docker run -d --name x5  --cpuset-cpus=0 --cpu-shares=20  -m 200m  alpine ping fb.com
  
```
