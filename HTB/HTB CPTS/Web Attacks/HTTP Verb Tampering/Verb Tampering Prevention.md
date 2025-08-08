Problem: limiting a page's authorization to a particular set of HTTP verbs/methods, which leaves the other remaining methods unprotected.

Example vulnerable configuration (Apache):
```xml
<Directory "/var/www/html/admin">
    AuthType Basic
    AuthName "Admin Panel"
    AuthUserFile /etc/apache2/.htpasswd
    <Limit GET>
        Require valid-user
    </Limit>
</Directory>
```
- Setting the authorization configurations for the admin web directory
- As the <Limit GET> keyword is being used, the Require valid-user setting will only apply GET requests, leaving it vulnerable to POST, HEAD, or OPTIONS requests.

Same vulnerability in tomcat:
```xml
<security-constraint>
    <web-resource-collection>
        <url-pattern>/admin/*</url-pattern>
        <http-method>GET</http-method>
    </web-resource-collection>
    <auth-constraint>
        <role-name>admin</role-name>
    </auth-constraint>
</security-constraint>
```

Same vulnerability in ASP.NET:
```xml
<system.web>
    <authorization>
        <allow verbs="GET" roles="admin">
            <deny verbs="GET" users="*">
        </deny>
        </allow>
    </authorization>
</system.web>
```

Use:
Apache: LimitExcept
Tomcat: http-method-omission
ASP.NET: add/remove

Consider disabling/denying all HEAD requests


## Insecure Coding

Vulnerable code:
```shell-session
if (isset($_REQUEST['filename'])) {
    if (!preg_match('/[^A-Za-z0-9. _-]/', $_POST['filename'])) {
        system("touch " . $_REQUEST['filename']);
    } else {
        echo "Malicious Request Denied!";
    }
}
```
- Inconsistent use of HTTP methods
- preg_match checks for special characters in POST, however the final system command uses the REQUEST variable, which covers both GET and POST parameters.


Use consistent use of HTTP methods
Expand the scope of testing in security filters by testing all request parameters. This can be done with:
PHP: $_REQUEST['param']
Java: request.getParameter('param')
C#: Request['param']

