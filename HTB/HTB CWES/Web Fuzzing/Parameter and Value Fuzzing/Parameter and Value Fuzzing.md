```
curl http://83.136.254.84:45169/get.php

Invalid parameter value
x: 
```
- We know that x is a parameter, now let's fuzz

```
curl http://IP:PORT/get.php?x=1

Invalid parameter value
x: 1
```
- 1 is invalid

```
wenum -w /usr/share/seclists/Discovery/Web-Content/common.txt --hc 404 -u "http://IP:PORT/get.php?x=FUZZ"
```
hc hides responses with the status code indicated


## POST
```
curl -d "" http://83.136.254.84:45169/post.php

Invalid parameter value
y: 
```
- y is a parameter, now let's fuzz

```
ffuf -u http://83.136.254.84:45169/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w /usr/share/seclists/Discovery/Web-Content/common.txt -mc 200 -v
```

```
curl -d "y=SU..." http://IP:PORT/post.php
```
