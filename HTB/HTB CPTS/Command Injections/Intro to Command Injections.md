User-controlled input to execute a system command on the back-end server
- OS Command Injection: occurs when user input is directly used as part of an OS command
- Code Injection: occurs when user input is directly within a function that evaluates code
- SQL Injections: occurs when user input is directly used as part of an SQL query
- Cross-Site Scripting/HTML Injection: occurs when exact user input is displayed on a web page

Other injection types:
- LDAP injection
- NoSQL Injection
- HTTP Header Injection
- XPath Injection
- IMAP Injection
- ORM Injection

## OS Command Injections

**PHP:
- exec
- system
- shell_exec
- passthru
- popen
```php
<?php
if (isset($_GET['filename'])) {
    system("touch /tmp/" . $_GET['filename'] . ".pdf");
}
?>
```
- filename is used directly with the touch command without being sanitized or escaped first

**NodeJS
- child_process.exec
- child_process.spawn
```javascript
app.get("/createfile", function(req, res){
    child_process.exec(`touch /tmp/${req.query.filename}.txt`);
})
```
- Same thing as php; filename parameter is used directly with touch without sanitizing it first


