
SMB
- smbclient
- crackmapexec
- smbmap
- impacket
- psexec.py
- smbexec.py
NFS
FTP
- ftp
- lftp
- ncftp
- filezilla
- crossftp
TFTP
SFTP
Email
- Thunderbird
- Claws
- Geary
- MailSpring
- mutt
- mailutils
- sendEmail
- swaks
- sendmail
Databases
- mssqli-cli
- mycli
- mssqlclient.py
- dbeaver
- MySQL Workbench
- SQL Server Management Studio or SSMS

Cloud:
Dropbox
Google Drive
OneDrive
SharePoint
AWS S3
Azure Blob Storage
Google Cloud Storage


##### SMB - Server Message Block

Windows
- [WINKEY] + [R]
- \\\192.168.220.129\Finance\
- dir n: /a-d /s /b | find /c ":\\"
(prints amount of files found)
- dir n:\\\*cred* /s /b
- dir n:\\\*secret* /s /b
- findstr /s /i cred n:\\\*.*

Windows CMD
- dir \\\192.168.220.129\Finance\

Windows CMD
- net use n: \\\192.168.220.129\Finance\ /user:plaintext Password123
(placed on n drive)

Windows PowerShell
- Get-ChildItem \\\192.168.220.129\Finance\
- New-PSDrive -Name "N" -Root "\\\192.168.220.129\Finance" -PSProvider "FileSystem"

Windows PowerShell - PSCredential Obejct
- $username = 'plaintext'
- $password = 'Password123'
- $secpassword = ConvertTo-SecureString $password -AsPlainText -Force
- $cred = New-Object System.Management.Automation.PSCredential $username, $secpassword
- New-PSDrive -Name "N" -Root "\\\192.168.220.129\Finance" -PSProvider "FileSystem" -Credential $cred

Windows PowerShell - GCI
- N:
- (Get-ChildItem -File -Recurse | Measure-Object).Count
- Get-ChildItem -Recurse -Path N:\\ -Include \*cred* -File

Windows PowerShell - Select-String
- Get-ChildItem -Recurse -Path N:\\ | Select-String "cred" -List


##### Linux

sudo apt install cifs-utils

Linux - Mount
- sudo mkdir /mnt/Finance
- sudo mount -t cifs -o username=plaintext,password=Password123!,domain=. //192.168.220.129/Finance /mnt/Finance
- mount -t cifs //192.168.220.129/Finance /mnt/Finance -o credentials=/path/credentialfile
	- Credential file:
	- username=plaintext
	- password=Password123
	- domain=.

Linux - Find
- find /mnt/Finance/ -name \*cred*
- grep -rn /mnt/Finance/ -ie cred


##### Email

sudo apt-get install evolution

Connect to IMAP and SMTP using Evolution:
https://www.youtube.com/watch?v=xelO2CiaSVs


##### MSSQL

Linux
```shell-session
sqsh -S 10.129.20.13 -U username -P Password123
```

Windows
```cmd-session
sqlcmd -S 10.129.20.13 -U username -P Password123
```


##### MySQL

Linux
```shell-session
mysql -u username -pPassword123 -h 10.129.20.13
```

Windows
```cmd-session
mysql.exe -u username -pPassword123 -h 10.129.20.13
```


Install dbeaver
```shell-session
sudo dpkg -i dbeaver-<version>.deb
```

Run dbeaver
```shell-session
dbeaver &
```

Connecting to MSSQL DB using dbeaver:
https://www.youtube.com/watch?v=gU6iQP5rFMw

Connecting to MySQL DB using dbeaver:
https://www.youtube.com/watch?v=PeuWmz8S6G8


Reasons we may not have access to the resource:
- authentication
- privileges
- network connection
- firewall rules
- protocol support