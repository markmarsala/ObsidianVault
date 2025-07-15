
![[Pasted image 20250302144318.png]]

Structure: 
```shell-session
<No.> <type>/<os>/<service>/<name>
```

```shell-session
use 0
```
```shell-session
options
```
```shell-session
info
```
```shell-session
set RHOSTS 10.10.10.40
```

```shell-session
setg RHOSTS 10.10.10.40
```
(permanent IP until program is restarted)

```shell-session
run
```



```shell-session
show targets
```
(operating systems which are vulnerable)

```shell-session
set target 6
```
(If set to automatic, it will perform service scan. If we know the OS version, we can set the target)

To identify a target correctly, we will need to:
- Obtain a copy of the target binaries
- Use msfpescan to locate a suitable return address