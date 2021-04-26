## Commonly used Docker Commands

### Listing the docker images in the local registry
docker images

### Pulling a image from Docker Hub(Remote Registry) to Local docker registry
docker pull hello-world:latest
docker pull ubuntu:16.04 

### Create a container in the foreground mode using. ubuntu:16.04 docker image
```
docker run -it --name server2 --hostname server2 ubuntu:16.04 /bin/bash
```

### Create container in the background mode out of ubuntu:16.04 docker image
```
docker run -dit --name server1 --hostname server1 ubuntu:16.04 /bin/bash
```
### Listing the currently running containers
```
docker ps
```

### Listing all containers
```
docker ps -a
```

### Stopping a running container
```
docker stop server1
```

### Starting a stopped container
```
docker start server1
```

### Restarting a container
```
docker restart server1
```
