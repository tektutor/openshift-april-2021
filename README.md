## Commonly used Docker Commands

### Create a container in the foreground mode using. ubuntu:16.04 docker image
```
docker run -it --name server2 --hostname server2 ubuntu:16.04 /bin/bash
```

### Create container in the background mode out of ubuntu:16.04 docker image
```
docker run -dit --name server1 --hostname server1 ubuntu:16.04 /bin/bash
```

