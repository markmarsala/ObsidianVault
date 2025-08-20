## Preventing Directory Traversal

If a user can control directory traversal, they can:
- Read /etc/passwd and potentially find SSH Keys or know valid user names for a password spray attack
- Find other services on the box such as Tomcat and read the tomcat-users.xml file
- Discover valid PHP Session Cookies and perform session hijacking
- Read current web application configuration and source code

Prevention:
- Use your programming language's built-in tool to pull only the filename
  - PHP: basename()
- Sanitization
```
while(substr_count($input, '../', 0)) {
    $input = str_replace('../', '', $input);
};
```

## Web Server Configuration

1. Set allow_url_fopen and allow_url_include to Off
2. Lock web applications to their root directory, like running the application within Docker
3. open_basedir = /var/www in php.ini
4. Disable dangerous modules, like PHP Expect mod_userdir.


## Web Application Firewall (WAF)

ModSecurity - permissive mode
