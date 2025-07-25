## Enumeration
https://github.com/rebootuser/LinEnum

- OS Version
- Kernel Version
- Running Services
- Installed Packages and Versions
- Logged in Users
- User Home Directories
- Sudo privileges
- Configuration Files
- Readable Shadow File
- Password Hashes in /etc/passwd
- Cron Jobs
- Unmounted File Systems and Additional Drives
- SETUID and SETGID Permissions
- Writeable Directories
- Writeable Files

**List Current Processes
```shell-session
ps aux | grep root
```

**List Current Processes
```shell-session
ps au
```

**Home Directory Contents
```shell-session
ls /home
```

**User's Home Directory Contents
```shell-session
ls -la /home/stacey.jenkins/
```

**SSH Directory Contents
```shell-session
ls -l ~/.ssh
```

**Bash History
```shell-session
history
```

**Sudo - List User's Privileges
```shell-session
sudo -l
```

**Passwd
```shell-session
cat /etc/passwd
```

**Cron Jobs
```shell-session
la -la /etc/cron.daily/
```

**File Systems & Additional Drives
```shell-session
lsblk
```

**Find Writeable Directories
```shell-session
find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null
```

**Find Writeable Files
```shell-session
find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null
```
