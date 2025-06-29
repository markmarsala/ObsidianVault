### MSSQL
--------
Microsoft SQL - TCP port 1433

SSMS - SQL Server Management Studio (similar to pgAdmin for PostgreSQL)

Clients:
- mssql-cli
- SQL Server Powershell
- HeidiSQL
- SQLPro
- Impacket's mssqlclient.py

Find clients: 
```shell-session
locate mssqlclient
```

Default runs as NT SERVICE\MSSQLSERVER

Uses Windows Authentication (local Security Accounts Manager (SAM) database or the domain controller). Encryption is not enforced by default.

Nmap scripts:
- ms-sql-info
- ms-sql-empty-password
- ms-sql-xp-cmdshell
- ms-sql-config
- ms-sql-ntlm-info
- ms-sql-tables
- ms-sql-hasdbaccess
- ms-sql-dac
- ms-sql-dump-hashes

```shell-session
sudo nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 10.129.201.248
```

Metasploit module:
- scanner/mssql/mssql_ping

Connect:
```shell-session
python3 mssqlclient.py Administrator@10.129.201.248 -windows-auth
```

Commands:
- select name from sys.databases
- use [database]
- select name from sys.tables
- select * from [table]

