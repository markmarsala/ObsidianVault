```
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt -u http://admin.academy.htb:37495/admin/admin.php?FUZZ=key -ic -c -fs 798

ffuf -w /opt/useful/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx

curl http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'id=key' -H 'Content-Type: application/x-www-form-urlencoded'

ffuf -w nums.txt -X POST -d "id=FUZZ" -H "Content-Type: application/x-www-form-urlencoded" -u http://admin.academy.htb:37495/admin/admin.php -ic -c -fs 768
```
