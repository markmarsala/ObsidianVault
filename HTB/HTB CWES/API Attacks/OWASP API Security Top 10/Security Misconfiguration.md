CWE-89: Improper Neutralization of Special Elements used in an SQL Command ('SQL Injection')

```
htbpentester12@pentestercompany.com:HTBPentester12
```
Roles:
- Products_GetProductsTotalCountByNameSubstring
Endpoints:
- /api/v1/products/{Name}/count

```
laptop' OR 1=1 --
```


## HTTP Headers
If an API does not set a secure Access-Control-Allow-Origin as part of its CORS (Cross-Origin Resource Sharing), it may be vulnerable to CSRF.


**Prevention
- Parameterized queries or an Object Relational Mapper.
- Validate user-controlled input
- OWASP secure headers: https://github.com/OWASP/www-project-secure-headers
