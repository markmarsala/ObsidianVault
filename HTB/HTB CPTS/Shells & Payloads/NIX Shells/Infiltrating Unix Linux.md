
Metasploit modules that aren't found in the search can be found here: https://github.com/rapid7/metasploit-framework/tree/master/modules/exploits

apt update; apt install metasploit-framework

Use Nmap to enumerate
Check versions of tech stack
Check exploits associated with service

rConfig 3.9.6
```shell-session
linux/http/rconfig_vendors_auth_file_upload_rce
```

If given a shell with no prompt, we can import one with python.

```shell-session
which python 
```
```shell-session
python -c 'import pty; pty.spawn("/bin/sh")' 
```

