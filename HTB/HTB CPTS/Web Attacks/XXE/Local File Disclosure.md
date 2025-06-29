#### Local File Disclosure

1. We must find web pages that accept an XML user input.
	1. Intercept the HTTP request with Burp to see if it shows XML
2. If application prints the inputted data to the screen and is using outdated XML libraries, or does not apply filters or sanitization, it could be vulnerable to local file disclosure.
3. Input a new entity right after the XML Declaration. Reference it in the XML doc:
![[Pasted image 20250209150105.png]]
If the value is returned in the response (Inlane Freight), we can inject XML code. A non-vulnerable application would return "&company;"

Note: Some web applications may default to a JSON format in HTTP request, but may still accept other formats, including XML. So, even if a web app sends requests in a JSON format, we can try changing the Content-Type header to application/xml, and then convert the JSON data to XML with an online tool. If the web application does accept the request with XML data, then we may also test it against XXE vulnerabilities, which may reveal an unanticipated XXE vulnerability.
