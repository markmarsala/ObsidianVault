Questions:
- What services and applications are installed?
- What services are running?
- What sockets are in use?
- What users, admins, and groups exist on the system?
- Who is current logged in? What users recently logged in?
- What password policies, if any, are enforced on the host?
- Is the host joined to an Active Directory domain?
- What types of interesting information can we find in history, log, and backup files
- Which files have been modified recently and how often? Are there any interesting patterns in file modification that could indicate a cron job in use that we may be able to hijack?
- Current IP addressing information
- Anything interesting in the /etc/hosts file?
- Are there any interesting network connections to other systems in the internal network or even outside the network?
- What tools are installed on the system that we may be able to take advantage of? (Netcat, Perl, Python, Ruby, Nmap, tcpdump, gcc, etc.)
- Can we access the bash_history file for any users and can we uncover any thing interesting from their recorded command line history such as passwords?
- Are any Cron jobs running on the system that we may be able to hijack?

## Internals

**Network Interfaces
```shell-session
ip a
```

**Hosts
```shell-session
cat /etc/hosts
```

**User's last login
```shell-session
lastlog
```

**Logged In Users
```shell-session
w
```

**Command History
```shell-session
history
```

**Finding History Files
```shell-session
find / -type f \( -name *_hist -o -name *_history \) -exec ls -l {} \; 2>/dev/null
```

**Cron
```shell-session
ls -la /etc/cron.daily/
```

**Proc (access process information)
```shell-session
find /proc -name cmdline -exec cat {} \; 2>/dev/null | tr " " "\n"
```

## Services

**Installed Packages
```shell-session
apt list --installed | tr "/" " " | cut -d" " -f1,3 | sed 's/[0-9]://g' | tee -a installed_pkgs.list
```

**Sudo Version
```shell-session
sudo -V
```

**Binaries
```shell-session
ls -l /bin /usr/bin/ /usr/sbin/
```

**GTFObins
```shell-session
for i in $(curl -s https://gtfobins.github.io/ | html2text | cut -d" " -f1 | sed '/^[[:space:]]*$/d');do if grep -q "$i" installed_pkgs.list;then echo "Check GTFO for: $i";fi;done
```

**Trace System Calls
```shell-session
strace ping -c1 10.129.112.20
```

**Configuration Files
```shell-session
find / -type f \( -name *.conf -o -name *.config \) -exec ls -l {} \; 2>/dev/null
```

**Scripts
```shell-session
find / -type f -name "*.sh" 2>/dev/null | grep -v "src\|snap\|share"
```

**Running Services by User
```shell-session
ps aux | grep root
```
