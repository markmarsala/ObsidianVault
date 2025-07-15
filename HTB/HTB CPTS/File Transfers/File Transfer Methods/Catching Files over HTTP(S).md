
#### Nginx - Enabling PUT

Create a Directory to Handle Uploaded Files
```shell-session
sudo mkdir -p /var/www/uploads/SecretUploadDirectory
```

Change the Owner to www-data
```shell-session
sudo chown -R www-data:www-data /var/www/uploads/SecretUploadDirectory
```

Create Nginx Configuration File
/etc/nginx/sites-available/upload.conf
```shell-session
server {
    listen 9001;
    
    location /SecretUploadDirectory/ {
        root    /var/www/uploads;
        dav_methods PUT;
    }
}
```

Symlink our Site to the sites-enabled Directory
```shell-session
sudo ln -s /etc/nginx/sites-available/upload.conf /etc/nginx/sites-enabled/
```

Start Nginx
```shell-session
 sudo systemctl restart nginx.service
```

Verifying Errors
```shell-session
tail -2 /var/log/nginx/error.log
```

```shell-session
ss -lnpt | grep 80
```

```shell-session
ps -ef | grep 2811
```

Remove NginxDefault Configuration
```shell-session
sudo rm /etc/nginx/sites-enabled/default
```

Upload File using cURL
```shell-session
curl -T /etc/passwd http://localhost:9001/SecretUploadDirectory/users.txt
```

```shell-session
sudo tail -1 /var/www/uploads/SecretUploadDirectory/users.txt 
```

Ensure directory listing is not enabled by navigating to
http://localhost/SecretUploadDirectory