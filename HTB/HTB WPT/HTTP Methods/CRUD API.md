## APIs
```
curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london ...SNIP...
```
- update city table with row called london

CRUD:
- Create (POST)
- Read (GET)
- Update (PUT)
- Delete (DELETE)

**Read
```
curl -s http://<SERVER_IP>:<PORT>/api.php/city | jq
```
- jq properly formats JSON


**Create
```
curl -X POST http://<SERVER_IP>:<PORT>/api.php/city/ -d '{"city_name":"HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'
curl -s http://<SERVER_IP>:<PORT>/api.php/city/HTB_City | jq
```

**Update (PATCH updates objects, but only certain data, like only city_name)
```
curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london -d '{"city_name":"New_HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json'
curl -s http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City | jq
```

**Delete
```
curl -X DELETE http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City
curl -s http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City | jq
```


