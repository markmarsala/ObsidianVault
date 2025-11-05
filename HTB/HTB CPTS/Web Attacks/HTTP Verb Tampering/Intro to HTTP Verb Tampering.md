Use a misconfiguration in either the back-end web server or the web application with HTTP verbs to acess information

HTTP Verbs:
- GET
- HEAD
- POST
- PUT
- DELETE
- CONNECT
- OPTIONS
- TRACE
- PATCH
- TRACK

## Insecure Configurations
For example, a system admin may use the following configuration to require authentication on a particular web page:
```XML
<Limit GET POST>
    Require valid-user
</Limit>
```
- Attacker can use HEAD to bypass authentication machanism

## Insecure Coding
For example, if a web page was found to be vulnerble to a SQL Injection vulnerability, and the back-end developer mitigated the SQL Injection vulnerability by the following applying input sanitization filters:
```shell-session
$pattern = "/^[A-Za-z\s]+$/";

if(preg_match($pattern, $_GET["code"])) {
    $query = "Select * from ports where port_code like '%" . $_REQUEST["code"] . "%'";
    ...SNIP...
}
```
- Sanitization is only be tested on GET
- Attacker may use a POST request to perform SQL injection
