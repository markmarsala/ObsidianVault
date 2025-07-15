
Tools:
- Wireshark
- Pcredz


##### Wireshark filters
- ip.addr == 56.48.210.13
- tcp.port == 80
- http
- dns
- tcp.flags.syn == 1 && tcp.flags.ack == 0
- icmp
- http.request.method == "POST"
- tcp.stream eq 53
- eth.addr == 00:11:22:33:44:55
- ip.src == 192.168.24.3 && ip.dst == 56.48.210.3


##### Pcredz

- Credit card numbers
- POP credentials
- SMTP credentials
- IMAP credentials
- SNMP community strings
- FTP credentials
- Credentials from HTTP NTLM/Basic headers, as well as HTTP Forms
- NTLMv1/v2 hashes from various traffic including DCE-RPC, SMBv1/2, LDAP, MSSQL, and HTTP
- Kerberos (AS-REQ Pre-Auth etype 23) hashes

```shell-session
./Pcredz -f demo.pcapng -t -v
```
