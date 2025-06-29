### NMAP
--------

-sn                              disables port scanning
-iL                               input file to scan 
-PE                 ping scan 'ICMP Echo requests' against the target
--packet-trace           shows all packets sent and received
--reason                     displays the reason for specific result
--disable-arp-ping 
--top-ports=10
-n             disables DNS resolution
-sT           TCP connect scan
-sS           TCP SYN scan
-sA           TCP ACK scan
-Pn           deactivates ICMP echo requests
-sU           UDP scan
-F             scans top 100 ports
-sV           version detection
-oN          outputs as .nmap
-oG          outputs with .gnmap
-oX          outputs with .xml
-oA          outputs all three formats
```shell-session
xsltproc target.xml -o target.html 
```
^ converts xml to html


[SPACE BAR]               view progress
--stats-every=5s        view progress every 5 seconds
-v                                verbosity
-vv                               vverbosity
-sC                              scripts
--script                       specific script category
-A                               aggressive scan (service detection, OS detection, traceroute)
-O                              OS detection
--traceroute


Powerful scripts (--script)
- auth
- broadcast
- brute
- default
- discovery
- dos
- exploit
- external
- fuzzer
- intrusive
- malware
- safe
- version
- vuln
- banner
- smtp-commands

#### Optimized RTT
--initial-rtt-timeout 50ms --max-rtt-timeout 100ms
(can overlook hosts)

-T <0-5>                           how fast to scan
--min-parallelism              frequency
--max-rtt-timeout             which timeouts the packets should have
--min-rate             num packets sent simultaneously
--max-retries         num retries for the scanned ports (default 10)
-D  RND:5                        decoy scan with 5 random IP addresses
-S                          scans using different source IP address
--dns-server          specifies the DNS servers ourselves (DNS proxying)
--script-trace


#### Firewall and IDS/IPS Evasion

- fragmentation of packets
- decoys
- Virtual Private Server (IP) with different IP addresses to detect IPS.