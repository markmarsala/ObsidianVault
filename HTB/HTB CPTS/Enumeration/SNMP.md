### SNMP
-----------
Simple Network Management Protocol
UDP port 161 and 162

Monitor network devices.

MIB - Management Information Base: stores devices information
OID - Object Identifier
Abstract Syntax Notation One

Config: /etc/snmp/snmpd.conf

Tools to footprint:
- snmpwalk (query the OIDs with their information)
```shell-session
snmpwalk -v2c -c public 10.129.14.128
```
- onesixtyone (brute-force community strings) w/ SecLists and crunch
```shell-session
sudo apt install onesixtyone
```
```shell-session
onesixtyone -c /opt/useful/seclists/Discovery/SNMP/snmp.txt 10.129.14.128
```
- braa (brute-force individual OIDs)
```shell-session
sudo apt install braa
```
```shell-session
braa <community string>@<IP>:.1.3.6.*      #syntax
```
```shell-session
braa public@10.129.14.128:.1.3.6.*
```

