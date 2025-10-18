Snaffler: https://github.com/SnaffCon/Snaffler

Windows Privilege Escalation Cheat Sheet: https://swisskyrepo.github.io/InternalAllTheThings/redteam/escalation/windows-privilege-escalation/

## Manually

**Search File Contents for Strings
```cmd
cd c:\Users\htb-student\Documents & findstr /SI /M "password" *.xml *.ini *.txt
findstr /si password *.xml *.ini *.txt *.config
findstr /spin "password" *.*
```
```ps
select-string -Path C:\Users\htb-student\Documents\*.txt -Pattern password
```

**Search for File Extensions
```cmd
dir /S /B *pass*.txt == *pass*.xml == *pass*.ini == *cred* == *vnc* == *.config*
where /R C:\ *.config
```
```ps
Get-ChildItem C:\ -Recurse -Include *.rdp, *.config, *.vnc, *.cred -ErrorAction Ignore
```


## Sticky Notes Passwords

```ps
ls C:\Users\user\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState
```
- Download the plum.sqlite* files to attack host
- Open in DB BRowser for SQLite
```sql
select Text from Note;
```

**Viewing Sticky Notes Data Using PowerShell

https://github.com/RamblingCookieMonster/PSSQLite

```ps
Set-ExecutionPolicy Bypass -Scope Process
cd .\PSSQLite\
Import-Module .\PSSQLite.psd1
$db = 'C:\Users\htb-student\AppData\Local\Packages\Microsoft.MicrosoftStickyNotes_8wekyb3d8bbwe\LocalState\plum.sqlite'
Invoke-SqliteQuery -Database $db -Query "SELECT Text FROM Note" | ft -wrap
```

**Strings to View DB File Contents
```
strings plum.sqlite-wal
```
- Transfer files to attack host and then run strings

## Other Files of Interest

**Other Interesting Files
```
%SYSTEMDRIVE%\pagefile.sys
%WINDIR%\debug\NetSetup.log
%WINDIR%\repair\sam
%WINDIR%\repair\system
%WINDIR%\repair\software, %WINDIR%\repair\security
%WINDIR%\iis6.log
%WINDIR%\system32\config\AppEvent.Evt
%WINDIR%\system32\config\SecEvent.Evt
%WINDIR%\system32\config\default.sav
%WINDIR%\system32\config\security.sav
%WINDIR%\system32\config\software.sav
%WINDIR%\system32\config\system.sav
%WINDIR%\system32\CCM\logs\*.log
%USERPROFILE%\ntuser.dat
%USERPROFILE%\LocalS~1\Tempor~1\Content.IE5\index.dat
%WINDIR%\System32\drivers\etc\hosts
C:\ProgramData\Configs\*
C:\Program Files\Windows PowerShell\*
```
