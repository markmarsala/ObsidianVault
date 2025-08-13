A type of validation on the back-end

Common forms of validating a file extension on the back-end:
- Testing against a blacklist of types
- Testing against a whitelist of types

```php
$fileName = basename($_FILES["uploadFile"]["name"]);
$extension = pathinfo($fileName, PATHINFO_EXTENSION);
$blacklist = array('php', 'php7', 'phps');

if (in_array($extension, $blacklist)) {
    echo "File type not allowed";
    die();
}
```
- This blacklist is not comprehensive. We can still upload a php shell with a different extension
- Case sensitive, but servers are case insensitive, meaning we can upload a .PHP file

## Fuzzing Extensions
1. Burp Intruder
2. Use: https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/web-extensions.txt
3. For PHP, use: https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Upload%20Insecure%20Files/Extension%20PHP/extensions.lst
4. Un-tick URL Encoding to avoid encoding the '.' before the extension

## Non-Blacklisted Extensions
1. .phtml passed, so right-click and send to repeater
