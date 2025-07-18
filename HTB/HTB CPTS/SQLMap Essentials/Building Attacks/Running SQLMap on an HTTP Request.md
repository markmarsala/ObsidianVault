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

## Full HTTP Requests (can be JSON or XML as well)
```shell-session
sqlmap -r req.txt
```
- In order to get the full HTTP request, use Burp and write it to a file, or with your browser, do Copy > Copy Request Headers
- Similar to the --data option, we can specify the parameter we want to inject in with an \*, such as '/id=*'

## Custom SQLMap Requests
```shell-session
sqlmap ... -H='Cookie:PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'
```
```shell-session
sqlmap ... --cookie='PHPSESSID=ab4530f4a7d10448457fa8b0eadac29c'
```
- '--host'
- '--referer'
- '-A/--user-agent'
- '--random-agent'
- '--mobile'

We can also try to inject the HTTP header values by putting in the custom injection mark (e.g. --cookie="id=1*")

**PUT
```shell-session
sqlmap -u www.target.com --data='id=1' --method PUT
```

## Dump database
- '--batch --dump'
