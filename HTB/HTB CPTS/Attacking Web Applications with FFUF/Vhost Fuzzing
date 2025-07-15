Vhost Fuzzing

Vhosts vs. sub-domains

Vhosts allow a server(1 IP address) to run multiple websites. They may or may not have public DNS records.


ffuf -w /opt/useful/seclists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb'

* We will always get a Status 200. If the VHost does exist, then it will have a different response size.
