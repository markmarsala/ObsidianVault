```
curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/ -i
```
- X changes the method
- L follows the redirection
- i checks the response


## Authenticated Cookies

```
curl -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/
```
- b sets the cookie

```
curl -H 'Cookie: PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/
```
- sets the cookie header


## JSON Data

The search form sends a POST request to search.php with the following data:
{"search":"london"}
```
curl -X POST -d '{"search":"london"}' -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' -H 'Content-Type: application/json' http://<SERVER_IP>:<PORT>/search.php
```
