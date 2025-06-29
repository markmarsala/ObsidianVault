### XSS Prevention

- Input sanitization
- Input validation
- Output sanitization
- Output validation (HTML encoding)
- Secure configuration
- WAF

XSS vulnerabilities are mainly linked to:
- Source: a user input field
- Sink: displays the input field
#### Front-end

Validate:
```javascript
function validateEmail(email) {
    const re = /^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
    return re.test($("#login input[name=email]").val());
}
```

Do not allow JavaScript by escaping any special characters:
```javascript
<script type="text/javascript" src="dist/purify.min.js"></script>
let clean = DOMPurify.sanitize( dirty );
```
This prevents DOM XSS

##### Direct Input

Never use user input directly within certain HTML tags:
- JavaScript code <script></script>
- CSS Style Code <style></style>
- Tag/Attribute Fields <div name='INPUT'></div>
- HTML Comments <!-- -->

Do not use these JavaScript functions that allow changing raw text of HTML fields:
- DOM.innerHTML
- DOM.outerHTML
- document.write()
- document.writeln()
- document.domain

And do not use these jQuery functions:
- html()
- parseHTML()
- add()
- append()
- prepend()
- after()
- insertAfter()
- before()
- insertBefore()
- replaceAll()
- replaceWith()


#### Back-end

Prevent stored and reflected XSS

Prevention:
- User input and output sanitization and validation
- Server configuration
- Back-end tools

Input Validation:
```php
if (filter_var($_GET['email'], FILTER_VALIDATE_EMAIL)) {
    // do task
} else {
    // reject input - do not display it
}
```

Input Sanitization:
PHP backend
```php
addslashes($_GET['email'])
```

NodeJS:
```javascript
import DOMPurify from 'dompurify';
var clean = DOMPurify.sanitize(dirty);
```

Output HTML encoding:
PHP
```php
htmlentities($_GET['email']);
```
```php
htmlspecialchars($_GET['email']);
```

NodeJS
```javascript
import encode from 'html-entities';
encode('<'); // -> '&lt;'
```


##### Server Configuration
- Using HTTPS across the entire domain
- Using XSS prevention headers
- Using the appropraite Content-Type for the page, like X-Content-Type-Options=nosniff
- Using Content-Security-Policy options, like script-src 'self', which only allows locally hosted scripts
- Using the HttpOnly and Secure cookie flags to prevent JavaScript from reading cookies and only transport them over HTTPS.


WAF

Frameworks with built-in XSS protection: ASP.NET

