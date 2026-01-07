**Download file
```
curl -s -O inlanefreight.com/index.html
```

**Skip HTTPS (SSL) certificate validation
```
curl -k https://inlanefreight.com
```

**Print full HTTP request/response details
```
curl inlanefreight.com -v
```

**HEAD request
```
curl -I inlanefreight.com
```

**Print response headers and response body
```
curl -i inlanefreight.com
```

**Set User-Agent header
```
curl https://www.inlanefreight.com -A 'Mozilla/5.0'
```

**Set HTTP basic authorization credentials
```
curl -u admin:admin http://<SERVER_IP>:<PORT>/
```

**Pass GET parameters
```
curl 'http://<SERVER_IP>:<PORT>/search.php?search=le'
```

**Set request header
```
curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<SERVER_IP>:<PORT>/
```

**Set request cookies
```
curl -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/
```

**Send POST request with JSON data
```
curl -X POST -d '{"search":"london"}' -H 'Content-Type: application/json' http://<SERVER_IP>:<PORT>/search.php
```

## APIs

**Read entry
```
 curl http://<SERVER_IP>:<PORT>/api.php/city/london
```

**Read all entries
```
curl -s http://<SERVER_IP>:<PORT>/api.php/city/ | jq
```

**Create (add) entry
```
curl -X POST http://<SERVER_IP>:<PORT>/api.php/city/ -d '{"city_name":"HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'
```

**Update (modify) entry
```
curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london -d '{"city_name":"New_HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'
```

**Delete Entry
```
curl -X DELETE http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City
```

## Browser DevTools

**devtools
```
[CTRL+SHIFT+I] or [F12]
```

**network tab
```
[CTRL+SHIFT+E]
```

**console tab
```
[CTRL+SHIFT+K]
```
