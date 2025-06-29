
TCP/21

Commands:
- ls
- cd
- get
- mget (multiple files)
- put
- mput (multiple files)
- help

```shell-session
sudo nmap -sC -sV -p 21 192.168.2.142 
```

Anonymous Authentication
```shell-session
ftp 192.168.2.142
```


Password spraying > brute forcing


Brute Forcing with Medusa
```shell-session
medusa -u fiona -P /usr/share/wordlists/rockyou.txt -h 10.129.203.7 -M ftp
```

FTP bounce - get information about internal devices not publicly accessible
![[Pasted image 20250415130233.png]]

FTP Bounce (-b)
```shell-session
nmap -Pn -v -n -p80 -b anonymous:password@10.10.110.213 172.17.0.2
```




