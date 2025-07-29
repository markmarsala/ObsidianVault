- rbash
- restricted-access shell
- rksh
- rzsh

## Escaping

**Command injection
```shell-session
ls -l `pwd`
```

**Command Substitution
- Use `` to execute commands

**Command Chaining
- Use shell metacharacter (;) or (|)

**Environment Variables
- Modify value of environment variable to specify a different directory

**Shell Functions
- Define a shell functions that execute a command


```shell-session
while read line; do echo "$line"; done < flag.txt
```
