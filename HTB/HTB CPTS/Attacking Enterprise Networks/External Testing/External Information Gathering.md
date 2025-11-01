Perform a banner grab of the services listening on the target host and find a non-standard service banner. Submit the name as your answer (format: word_word_word)
```
sudo nmap --open -oA inlanefreight_ept_tcp_1k -iL scope
sudo nmap --open -p- -A -oA inlanefreight_ept_tcp_all_svc -iL scope
egrep -v "^#|Status: Up" discovery_agg.gnmap | cut -d ' ' -f4- | tr ',' '\n' | sed -e 's/^[ \t]*//' | awk -F '/' '{print $7}' | grep -v "^$" | sort | uniq -c | sort -k 1 -nr
```

Perform a DNS Zone Transfer against the target and find a flag. Submit the flag value as your answer (flag format: HTB{ }).
```
dig axfr inlanefreight.local @10.129.229.147
```
- https://dnsdumpster.com/

What is the FQDN of the associated subdomain?
```
flag.inlanefreight.local
```

Perform vhost discovery. What additional vhost exists? (one word)
```
gobuster vhost -u http://inlanefreight.local -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain
curl -s -I http://10.129.203.101 -H "HOST: defnotvalid.inlanefreight.local" | grep "Content-Length:"
ffuf -w namelist.txt:FUZZ -u http://10.129.203.101/ -H 'Host:FUZZ.inlanefreight.local' -fs 15157
```
- Add these to /etc/hosts
