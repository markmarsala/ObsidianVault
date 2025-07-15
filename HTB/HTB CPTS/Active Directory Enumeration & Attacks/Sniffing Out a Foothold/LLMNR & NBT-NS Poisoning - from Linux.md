
Link-Local Multicast Name Resolution - LLMNR port 5355/UDP
NetBIOS Name Service - NBT-NS port 137/UDP

Windows host identification when DNS fails.

Can collect NTLMv1 and NTLMv2 password hashes.
- Crack offline with Hashcat or John

Tools for LLMNR & NBT-NS poisoning:
- Responder
- [Inveigh](https://github.com/Kevin-Robertson/Inveigh)
- Metasploit


**Responder
```shell-session
responder -h
```
- 'A' puts us in analyze mode
- '-wf' starts the WPAD rogue proxy server and attempt to fingerprint the remote host for operating system and version.
- '-v' is verbosity
- '-F' Force NTLM or basic authentication
- '-P' Force proxy authentication
- '-w' utilizes the built in WPAD proxy server (captures all HTTP request of any user that launch Internet Explorer if the browser has Auto-detect settings enabled)

*If we capture a hash, it will write to /usr/share/responder/logs in the format  `(MODULE_NAME)-(HASH_TYPE)-(CLIENT_IP).txt`*

- Run as root or sudo

```bash
sudo responder -I ens224 
```

**Hashcat
- hash mode 5600 for NTLMv2
```shell-session
hashcat -m 5600 forend_ntlmv2 /usr/share/wordlists/rockyou.txt
```