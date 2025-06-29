
Key Terms to Search
- Passwords
- Username
- Users
- Configuration
- pwd
- Passphrases
- User account
- Passkeys
- dbcredential
- Login
- Keys
- Creds
- Passphrases
- dbpassword
- Credentials

1. Windows Search

2. Lazagne: https://github.com/AlessandroZ/LaZagne/releases/
```cmd-session
start lazagne.exe all
```

3. findstr
```cmd-session
findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml *.git *.ps1 *.yml
```

- Passwords in Group Policy in the SYSVOL share
- Passwords in scripts in the SYSVOL share
- Password in scripts on IT shares
- Passwords in web.config giles on dev machines and IT shares
- unattend.xml
- Passwords in the AD user or computer description fields
- KeePass databases -> pull hash, crack and get loads of access
- Found on user systems and shares
- Files such as pass.txt, passwords.docx, passwords.xlsx found on user systems, shares, Sharepoint