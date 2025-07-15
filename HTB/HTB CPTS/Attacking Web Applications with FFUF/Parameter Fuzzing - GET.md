Keys are usually passed as a parameter using either a GET or POST HTTP request

Example of parameter passing:
- http://admin.academy.htb:PORT/admin/admin.php?param1=key

```shell-session
ffuf -w /opt/useful/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php?FUZZ=key -fs xxx
```
