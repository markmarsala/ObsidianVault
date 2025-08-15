## Filter/WAF Detection
If an error message displays a different page, with information like our IP and our request, this may indicate that it was denied by a WAF.

In the example, the "Invalid input" was displayed where the regular input goes, so the PHP application itself prevented it.
- Must be blacklisting!

## Blacklisted Characters
```php
$blacklist = ['&', '|', ';', ...SNIP...];
foreach ($blacklist as $character) {
    if (strpos($_POST['ip'], $character) !== false) {
        echo "Invalid input";
    }
}
```

1. Reduce to one character at a time and see when it gets blocked
