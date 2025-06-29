### IPMI
-----------
Intelligent Platform Management Interface - allows sysadmins to manage and monitor systems even if powered off or in an unresponsive state
Port 623 - UDP

Gaining access to a BMC (Base Management Controller) allows
- monitor
- reboot
- power off
- reinstall host operating system

BMCs:
- HP iLO
- Dell iDRAC
- Supermicro IPMI

nmap script:
- ipmi-version

metasploit module:
- auxiliary/scanner/ipmi/ipmi_version
- auxiliary/scanner/ipmi/ipmi_dumphashes

Crack these hashes offline with hashcat -m 7300 ^

HP iLO crack:
hashcat -m 7300 ipmi.txt -a 3 ?1?1?1?1?1?1?1?1 -1 ?d?u

Access to BMC web console = high risk finding

Password reuse = high risk finding if access to critical systems


