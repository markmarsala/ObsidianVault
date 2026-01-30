```
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://academy.htb:43220/ -H 'Host: FUZZ.academy.htb' -c -fs 985

sudo nano /etc/hosts
archive, test, faculty

ffuf -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt -u http://faculty.academy.htb:43220/indexFUZZ -ic -c -fs 985

ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt -u http://faculty.academy.htb:43220/courses/FUZZ.php7 -ic -c -fs 985

ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -u http://faculty.academy.htb:43220/courses/linux-security.php7?FUZZ=key -ic -c -fs 774

ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -H "Content-Type: application/x-www-form-urlencoded" -d "FUZZ=key" -u http://faculty.academy.htb:43220/courses/linux-security.php7 -ic -c -fs 774

curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "username=harry" http://faculty.academy.htb:43220/courses/linux-security.php7
```
- Check both GET and POST variables
- Check multiple extensions
