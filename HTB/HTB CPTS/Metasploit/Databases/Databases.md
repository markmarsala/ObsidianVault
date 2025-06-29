Metasploit works with PostgreSQL
```shell-session
sudo service postgresql status
```
```shell-session
sudo systemctl start postgresql
```

Initiate database
```shell-session
sudo msfdb init
```
(may encounter error, just apt update metasploit-framework)

If database is already configured:
```shell-session
sudo msfdb status
```

Connect:
```shell-session
sudo msfdb run
```

Reinitiate:
```shell-session
msfdb reinit
```
```shell-session
cp /usr/share/metasploit-framework/config/database.yml ~/.msf4/
```
```shell-session
sudo service postgresql restart
```
```shell-session
msfconsole -q
```

Database options:
```shell-session
help database
```
- db_import

View workspace:
```shell-session
workspace
```
- -a = add
- -d = delete
- [name] = switch workspaces
- -h = help

Importing scan results:
```shell-session
db_import Target.xml
```
```shell-session
hosts
```
```shell-session
hosts -h
```
```shell-session
services
```
```shell-session
services -h
```
```shell-session
creds
```
```shell-session
creds -h
```
```shell-session
loot
```
```shell-session
loot -h
```

Nmap scan from metasploit
```shell-session
db_nmap -sV -sS 10.10.10.8
```

Data Backup:
```shell-session
db_export -h
```
```shell-session
db_export -f xml backup.xml
```



