POST requests are not passed with the URL (GET is), so we cannot simply append a ? symbol.

POST requests are passed in the data field within the HTTP request.

In ffuf, use '-d' flag and '-X POST'

