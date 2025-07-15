POST requests are not passed with the URL (GET is), so we cannot simply append a ? symbol.

POST requests are passed in the data field within the HTTP request.

In ffuf, use '-d' flag and '-X POST'

```shell-session
ffuf -w /opt/useful/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```
- In PHP, POST data conent-tpe cn only accept application/x-www-form-urlencoded

