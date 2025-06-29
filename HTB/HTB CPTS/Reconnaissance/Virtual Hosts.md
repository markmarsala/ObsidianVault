##### Virtual Hosts

```apacheconf
# Example of name-based virtual host configuration in Apache
<VirtualHost *:80>
    ServerName www.example1.com
    DocumentRoot /var/www/example1
</VirtualHost>

<VirtualHost *:80>
    ServerName www.example2.org
    DocumentRoot /var/www/example2
</VirtualHost>

<VirtualHost *:80>
    ServerName www.another-example.net
    DocumentRoot /var/www/another-example
</VirtualHost>
```

1. Name-based virtual hosting
2. IP-based virtual hosting
3. Port-based virtual hosting

Virtual host discovery tools:
- gobuster
```shell-session
gobuster vhost -u http://<target_IP_address> -w <wordlist_file> --append-domain -t -k -o <output>
```
- feroxbuster
- ffuf

```shell-session
gobuster vhost -u http://inlanefreight.htb:81 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain
```

