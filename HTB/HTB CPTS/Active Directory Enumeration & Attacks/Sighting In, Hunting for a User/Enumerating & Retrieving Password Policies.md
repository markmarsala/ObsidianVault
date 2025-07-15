**Linux Credentialed
- CrackMapExec
- rpcclient
```shell-session
crackmapexec smb 172.16.5.5 -u avazquez -p Password123 --pass-pol
```


**Linux SMB NULL Sessions
- enum4linux
```shell-session
enum4linux -P 172.16.5.5
```
- enum4linux-ng (can output YAML or JSON)
```shell-session
enum4linux-ng -P 172.16.5.5 -oA ilfreight
```
```shell-session
cat ilfreight.json 
```
- CrackMapExec
- rpcclient
```shell-session
rpcclient -U "" -N 172.16.5.5
```
```shell-session
querydominfo
```


**Windows Null Session
```cmd-session
net use \\DC01\ipc$ "" /u:""
```
(Should print command completed successfully if possible)


**Linux LDAP Anonymous Bind
- ldapsearch
```-session
ldapsearch -H 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength
```


**Windows Password Policy Enumeration
- net.exe
```cmd-session
net accounts
```
- PowerView
```powershell-session
import-module .\PowerView.ps1
```
```powershell-session
Get-DomainPolicy
```


