Ways to transfer files:
- GitHub (cerutil: https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/certutil)
- Dropbox
- Google Drive
- FTP Server
- Impacket smbserver: https://github.com/fortra/impacket/blob/master/examples/smbserver.py

(PowerUp.ps1) PowerShell script to enumerate privilege escalation vectors: https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1

SeImpersonatePrivilege check: 
\> whoami /priv

If enabled, use PrintSpoofer tool: https://github.com/itm4n/PrintSpoofer


Obstacles in our way:
- Application Control Policy
- Web content filtering
- Network firewall
- Application whitelisting
- AV/EDR
- IDS/IPS
