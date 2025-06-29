
- No need for proxychains

```shell-session
sudo apt-get install sshuttle
```

```shell-session
sudo sshuttle -r ubuntu@10.129.202.64 172.16.5.0/23 -v
```
- -r means connect to a remote machine

```shell-session
nmap -v -sV -p3389 172.16.5.19 -A -Pn
```

