In browser > Network > Right click on GET request > copy as cURL

## Curl Commands
```shell-session
sqlmap 'http://www.example.com/?id=1' -H 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0' -H 'Accept: image/webp,*/*' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'Connection: keep-alive' -H 'DNT: 1'
```

## GET/POST Requests
```shell-session
sqlmap 'http://www.example.com/' --data 'uid=1&name=test'
```
- '--data' flag is used for testing POST data
- POST parameters uid and name will be tested for SQLi

```shell-session
sqlmap 'http://www.example.com/' --data 'uid=1*&name=test'
```
- If the uid is prone, label it with '*' special marker

