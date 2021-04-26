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
The above command creates a container by name server2 and start the container.

### Create container in the background mode out of ubuntu:16.04 docker image
```
docker run -dit --name server1 --hostname server1 ubuntu:16.04 /bin/bash
```
The above command creates a container by name server2 and start the container.

### Get inside a container
```
docker exec -it server1 bash
```
To come out of the secondary shell above, you may type exit.

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
Assuming a container by name server1 is already running.

### Starting a stopped container
```
docker start server1
```
Assuming a container by name server1 is in Exited state currently.

### Restarting a container
```
docker restart server1
```
