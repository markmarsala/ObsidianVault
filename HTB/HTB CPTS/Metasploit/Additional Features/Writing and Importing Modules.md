Loading modules at runtime
```shell-session
searchsploit nagios3
```
```shell-session
cp ~/Downloads/9861.rb /usr/share/metasploit-framework/modules/exploits/unix/webapp/nagios3_command_injection.rb
```
```shell-session
msfconsole -m /usr/share/metasploit-framework/modules/
```
```shell-session
loadpath /usr/share/metasploit-framework/modules/
```
```shell-session
reload_all
```


Download exploit from online

wget https://www.exploit-db.com/download/<exploit-id> -O exploit.rb

mv exploit.rb /usr/share/metasploit-framework/modules/exploits/custom/


msfconsole
reload_all

search custom

use exploit/custom/exploit