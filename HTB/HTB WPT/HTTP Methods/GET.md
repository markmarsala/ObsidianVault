## HTTP Basic Auth
```
curl -i http://<SERVER_IP>:<PORT>/

HTTP/1.1 401 Authorization Required
Date: Mon, 21 Feb 2022 13:11:46 GMT
Server: Apache/2.4.41 (Ubuntu)
Cache-Control: no-cache, must-revalidate, max-age=0
WWW-Authenticate: Basic realm="Access denied"
Content-Length: 13
Content-Type: text/html; charset=UTF-8

Access denied
```
- HTTP basic auth is in place since basic realm in the WWW-Authenticate header is present

```
curl -u admin:admin http://<SERVER_IP>:<PORT>/
curl http://admin:admin@<SERVER_IP>:<PORT>/
```
- u provides credentials
- both requests work the same

```
curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<SERVER_IP>:<PORT>/
```
- H sets the ehader directly (in this case, admin:admin in base64)

**Modern Method of Authorization
JWT
- Authroization would be of type Bearer and would contain a longer encrypted token

```
curl 'http://<SERVER_IP>:<PORT>/search.php?search=le' -H 'Authorization: Basic YWRtaW46YWRtaW4='
```
- We can also repeat the exact request right within the browser devtools, by selecting Copy>Copy as Fetch. This will copy the same HTTP request using the JavaScript Fetch library. Then, we can go to the JavaScript console tab by clicking [CTRL+SHIFT+K], paste our Fetch command and hit enter to send the request:

