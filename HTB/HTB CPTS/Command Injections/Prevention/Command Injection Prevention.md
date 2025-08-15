## System Commands
1. Instead of using system command execution functions, we should use built-in functions that perform the needed functionality, as back-end languages usually have secure implementations of these types of functionalities.
Example:
- Test whether a particular host is alive with PHP
- Use fsockopen

2. Never directly us user input with functions. Instead, validate and sanitize the user input on the back-end.

## Input Validation
1. Validate and sanitize user input both on the front-end and on the back-end.
- PHP has many built-in commands to check standard format
```php
if (filter_var($_GET['ip'], FILTER_VALIDATE_IP)) {
    // call function
} else {
    // deny request
}
```
2. Use regex with preg_match
```javascript
if(/^(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/.test(ip)){
    // call function
}
else{
    // deny request
}
```
- NodeJS is a back-end which contains libraries to validate, like 'is-ip' which can be downloaded with npm

## Input Sanitization
Removing any non-necessary special characters from the user input. Always performed after input validation.

**preg_replace
```php
$ip = preg_replace('/[^A-Za-z0-9.]/', '', $_GET['ip']);
```
- Only allows alphanumerical characters (A-Za-z0-9) and allows a . character

```javascript
var ip = ip.replace(/[^A-Za-z0-9.]/g, '');
```

Back-end javascript:
```javascript
import DOMPurify from 'dompurify';
var ip = DOMPurify.sanitize(ip);
```


## Server Configuration
- Use built-in WAF as well as external WAF
- Principle of Least Privilege (www-data)
- Disable functions (disable_functions=system,...)
- Limit scope accessible by the web application to its folder (open_basedir = 'var/www/html')
- Reject double-encoded requests and non-ASCII characters in URLs
- Avoid the use of sensitive/outdated libraries and modules

- 
