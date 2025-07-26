LinPEAS: https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS
LinEnum: https://github.com/rebootuser/LinEnum

- OS Version
- Kernel Version
- Running Services

## Gaining Situational Awareness
Basic commands:
- whoami
- id
- hostname
- ifconfig or ip a
- sudo -l

**Find OS Version
```shell-session
cat /etc/os-release
```

**Find PATH
```shell-session
echo $PATH
```

**Environment Variables
```shell-session
env
```

**Kernel version
```shell-session
uname -a
```

**CPU type/version
```shell-session
lscpu
```

**Login shell
```shell-session
cat /etc/shells
```

Defenses to check for:
- Exec Shield
- iptables
- AppArmor
- SELinux
- Fail2ban
- Snort
- Uncomplicated Firewall (ufw)

**Block devices (hard disks, USB drives, optical drives, etc.)
```shell-session
lsblk
```

**Print jobs
```shell-session
lpstat
```

**Mounted and unmounted drives
```shell-session
cat /etc/fstab
```

**Routing table
```shell-session
route
netstat -rn
```

**Internal DNS
```shell-session
/etc/resolv.conf
```

**Arp table
```shell-session
arp -a
```

**List users
```shell-session
cat /etc/passwd | cut -f1 -d:
```

Hash Algorithms
- Salted MD5
  - $1$
- SHA-256
  - $5$          
- SHA-512
  - $6$
- BCrypt
  - $2a$
- Scrypt
  - $7$
- Argon2
  - $argon2i$
 
**Check if user has logged in and has a shell
```shell-session
grep "sh$" /etc/passwd
```

**Existing Groups
```shell-session
cat /etc/group
```

**List members of any interesting groups
```shell-session
getent group sudo
```

**Check which users have a home directory
```shell-session
ls /home
```

**Mounted File Systems
```shell-session
df -h
```

**Unmounted File Systems
```shell-session
cat /etc/fstab | grep -v "#" | column -t
```

**All Hidden Files
```shell-session
find / -type f -name ".*" -exec ls -l {} \; 2>/dev/null | grep htb-student
```

**All Hidden Directories
```shell-session
find / -type d -name ".*" -ls 2>/dev/null
```
- /tmp files are retained for 10 days and deleted immediately when system is restarted
- /var/tmp are retained for 30 days and kept between reboots

**Temporary Files
```shell-session
ls -l /tmp /var/tmp /dev/shm
```



