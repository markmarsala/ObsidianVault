**--parse-errors
- Parse DBMS errors and display them as part of the program run

**-t
- Stores all sent and received HTTP requests
```shell-session
sqlmap -u "http://www.target.com/vuln.php?id=1" --batch -t /tmp/traffic.txt
```

## Verbose Output
**-v
```shell-session
sqlmap -u "http://www.target.com/vuln.php?id=1" -v 6 --batch
```

## Using Proxy
**--proxy
- Use with Burp to see all traffic
