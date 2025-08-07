First, we need to identify which pages are restricted by this authentication.
If we examine the HTTP request after clicking the Reset button or look at the URL that the button navigates to after clicking it, we see that it is at /admin/reset.php.
Either the /admin is restricted to authenticated users only, or only the /admin/reset.php page is.
We can confirm this by visiting the /admin directory, and we do indeed get prompted to log in again. /admin is restricted.

## Exploit
1. Identify the HTTP request method used by the web application using Burp Suite
2. Change method from GET to POST (right-click on intercepted request > Change Request Method > Forward)
3. 401 unauthorized error, meaning both GET and POST are configured

To see whether the server accepts HEAD requests, we can send an OPTIONS request to it and see what HTTP methods are accepted
```shell-session
curl -i -X OPTIONS http://SERVER_IP:PORT/
```
4. HEAD is accepted, so if we forward this, there is an empty output. If we type in the URL to go back to the application itself (http://IP:port), then we performed the intended functionality without authenticating!
