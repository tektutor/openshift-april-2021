## Commonly used Docker Commands

### Listing the docker images in the local registry
```
docker images
```

### Pulling an image from Docker Hub(Remote Registry) to Local docker registry
```
docker pull hello-world:latest
docker pull ubuntu:16.04
```

### Create a container in the foreground mode using ubuntu:16.04 docker image
```
docker run -it --name server2 --hostname server2 ubuntu:16.04 /bin/bash
```
The above command creates a container by name server2 and starts the container.

### Create container in the background mode using ubuntu:16.04 docker image
```
docker run -dit --name server1 --hostname server1 ubuntu:16.04 /bin/bash
```
The above command creates a container by name server2 and starts the container.

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
Assuming the server1 container is already created.

### Renaming a container
```
docker rename old-name new-name
```

### Connect local docker client to remote docker server
```
export DOCKER_HOST=tcp://10.20.30.40:4243
docker images
```
In the above IP 10.20.30.40 is the IP address of the Docker Server.
Port 4243 is where the REST API services are available from Docker Server.
'docker' is the client tool which connects to dockerd (Docker engine Server) running in a different machine.

### Building custom Docker Image

You need to create a file named Dockerfile with the below contents

```
FROM ubuntu:16.04
MAINTAINER Jeganathan Swaminathan <jegan@tektutor.org>

RUN apt-get update && apt-get install -y default-jdk maven git tree
```

Once the above file is saved, try the below command
```
docker build -t tektutor/custom-ubuntu .
```

### Containerizing python flask application

Create a Dockerfile as shown below
```
FROM python:3.8-slim-buster

COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt

COPY hello.py hello.py

CMD [ "python3", "hello.py" ]
```

Now you can build your custom python image as shown below
```
docker build -t tektutor/hello-python-ms:1.0 .
```

Create container using above image
```
docker run -d --name hello-python-ms --hostname hello-python-ms -p 5000:5000 tektutor/hello-python-ms:1.0 
docker run -d --name hello-python-ms --hostname hello-python-ms -p 5000:5000 tektutor/hello-python-ms:1.0
```
Verify if the container is up and running
```
docker ps
```

See if you can access the python microservice as shown below
```
curl http://localhost:5000
curl http://localhost:5000/tektutor
```

### Containerizing Java spring-boot microservice

Create a Dockerfile as shown below
```
FROM alpine:latest

RUN apk add openjdk8
COPY hello.jar hello.jar

CMD ["java", "-jar", "./hello.jar"]
```

Now you can build your custom python image as shown below
```
docker build -t tektutor/hello-java-ms:1.0 .
```

Create a container to test our custom Java microservice image
```
docker run -d --name hello-java-ms --hostname hello-java-ms -p 8080:8080 tektutor/hello-java-ms:1.0 .
```

See if you can access the microservice as shown below
```
curl http://localhost:8080/hello
```

### Port forwarding
```
docker run -d --name nginx1 --hostname nginx1 -p 8001:80 nginx:1.18
docker run -d --name nginx2 --hostname nginx2 -p 8002:80 nginx:1.18
docker run -d --name nginx3 --hostname nginx3 -p 8003:80 nginx:1.18
```

Check if the containers are running
```
docker ps
```
You may access the nginx web page as shown below
```
curl http://localhost:8001
curl http://localhost:8002
curl http://localhost:8003
```
### Creating mysql container without volume mounting
```
docker run -d --name mysql1 --hostname mysql1 -e MYSQL_ROOT_PASSWORD=root mysql:latest
```
See if the mysql container is running
```
docker ps
```
Assuming the mysql container is running,  try to get inside the container as shown below
```
docker exec -it mysql1 bash
```
From within mysql1 container, you may try using mysql client
```
mysql -u root -p
```
when mysql client prompts for password, type root as the password.
Once you are connect to the  mysql prompt,  you may try the below commands
```
SHOW DATABASES;
CREATE DATABASE tektutor;
USE tektutor;
CREATE TABLE training (id int, name varchar(25), duration varchar(10));
INSERT INTO training VALUES ( 1, "DevOps", "5 Days" );
INSERT INTO training VALUES ( 2, "OpenShift", "5 Days" );
INSERT INTO training VALUES ( 3, "Microservices", "5 Days" );

SELECT * FROM training;
```
After seeing the 3 saved records,  come out of the mysql prompt and then out of the container as shown below
```
exit
exit
```
We can now try stopping mysql container
```
docker stop mysql1
```
Try starting it again
```
docker start mysql1
```
Get inside the mysql1 container
```
docker exec -it mysql1  bash
```
Now try to connect to mysql server
```
mysql -u root -p
```
Type the password as root as password.
Now see if the tektutor database is still present after rebooting mysql1 container.
```
SHOW DATABASES;
```
NOW switch to tektutor database.
```
USE tektutor;
```
Now check if the table and records that we created are present
```
SELECT * FROM training;
```
From this exercise, you have understood container storage is not volatile.

Now let's come out of the mysql prompt and then out of the container.
```
mysql> exit
exit
```
Now you may remove the container
```
docker rm -f mysql1
```

### Volume mounting
```
docker run -d --name mysql1 --hostname mysql1 -v /tmp/data1:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root mysql:latest
docker run -d --name mysql2 --hostname mysql2 -v /tmp/data2:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root mysql:latest 
```
