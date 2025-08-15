Attacker can manipulate function parameters to display the content of any local file on the hosting server, leading to Local File Inclusion (LFI) vulnerability.

## Local File Inclusion (LFI)
Common place: templating engines

Template engines display common static parts: header, navigation bar, and footer.

Example: /index.php?page=about
- index.php sets static content
- We have control over the about portion of the request

## Examples of Vulnerable Code
- PHP
- NodeJS
- Java
- .Net

**PHP
Functions that may be vulnerable if user has control over path passed into them:
- include()
- include_once()
- require()
- require_once()
- file_get_contents()
```php
if (isset($_GET['language'])) {
    include($_GET['language']);
}
```
- If the path is taken from a user-controlled parameter, like a GET parameter, and the code does not explicitly filter and sanitize the user input, then the code becomes vulnerable to File Inclusion

**NodeJS
```javascript
if(req.query.language) {
    fs.readFile(path.join(__dirname, req.query.language), function (err, data) {
        res.write(data);
    });
}
```

```js
app.get("/about/:language", function(req, res) {
    res.render(`/${req.params.language}/about.html`);
});
```

**Java
```jsp
<c:if test="${not empty param.language}">
    <jsp:include file="<%= request.getParameter('language') %>" />
</c:if>
```
```jsp
<c:import url= "<%= request.getParameter('language') %>"/>
```

**.Net
```cs
@if (!string.IsNullOrEmpty(HttpContext.Request.Query['language'])) {
    <% Response.WriteFile("<% HttpContext.Request.Query['language'] %>"); %> 
}
```
```cs
@Html.Partial(HttpContext.Request.Query['language'])
```
```cs
<!--#include file="<% HttpContext.Request.Query['language'] %>"-->
```

## Read vs Execute
PHP
- include() / include_once()      Read/Execute/Remote URL
- require() / require_once()      Read/Execute
- file_get_contents()             Read/Remote URL
- fopen() / file()                Read
NodeJS
- fs.readFile()                   Read
- fs.sendFile()                   Read
- res.render()                    Read/Execute
Java
- include                         Read
- import                          Read/Execute/Remote URL
.NET
- @Html.Partial()                 Read
- @Html.RemotePartial()           Read/Remote URL
- Response.WriteFile()            Read
- include                         Read/Execute/Remote URL

