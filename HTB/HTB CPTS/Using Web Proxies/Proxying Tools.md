
## Proxychains

Set the '/etc/proxychains.conf' file
socks4         127.0.0.1 9050
http 127.0.0.1 8080

- Uncomment quiet_mode


## cURL
Usage: proxychains ...
```shell-session
proxychains curl http://SERVER_IP:PORT
```

## Nmap
```shell-session
nmap -h | grep -i prox
```
```shell-session
nmap --proxies http://127.0.0.1:8080 SERVER_IP -pPORT -Pn -sC
```

## msfconsole
```shell-session
msfconsole
use auxiliary/scanner/http/robots_txt
set PROXIES HTTP:127.0.0.1:8080
set RHOST SERVER_IP
set RPORT PORT
run
```

