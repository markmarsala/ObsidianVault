### MySQL
------
TCP port 3306

Nmap script
- --script mysql*

Connect
```shell-session
mysql -u root -pP4SSw0rd -h 10.129.14.128
```

Commands
- show databases;
- select version();
- use [database];
- show tables;
- show columns from [table];
- select host, unique_users from host_summary;
- select * from [table];
- select * from [table] where [column] = "string";

