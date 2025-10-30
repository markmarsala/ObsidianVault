Known Vulnerabilities:
1. CVE-2021-21087: Arbitrary disallow of uploading JSP source code
2. CVE-2020-24453: Active Directory integration misconfiguration
3. CVE-2020-24450: Command injection vulnerability
4. CVE-2020-24449: Arbitrary file reading vulnerability
5. CVE-2019-15909: Cross-Site Scripting (XSS) Vulnerability

Ports used: 80, 443, 1935, 25, 8500, 5500


## Enumeration

- Port Scanning: 80, 443
- File Extensions: .cfm or .cfc
- HTTP Headers: ColdFusion in headers
- Error Messages: ColdFusion specific tags
- Default Files: admin.cfm or CFIDE/administrator/index.cfm

**NMap ports and service scan results
```
nmap -p- -sC -Pn 10.129.247.30 --open
```

Navigate to http://10.129.247.30:8500/CFIDE/administrator/

