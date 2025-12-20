**After the configurations are transferred to the system, our client wants to know if it is possible to find out our target's DNS server version. Submit the DNS server version of the target as the answer.
```
sudo nmap -Pn --min-rate=500 --max-rtt-timeout 300ms -sU -sV -p 53 10.129.196.229
```
