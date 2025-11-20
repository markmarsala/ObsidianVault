1. Try directly accessing /admin.php
2. It redirects us to index.php, but still sends the admin html
3. Change the status code from 302 to 200 OK
  - Do intercept the response to this request
