### XML Enternal Entity (XXE) Injection

XML data is taken from a user-controlled input without properly sanitizing or safely parsing it.

##### XML
Extensible Markup Language is focused on storing documents; data and representing data structures.

Example:
![[Pasted image 20250209141839.png]]

Key elements of XML:
- Tag (<date></date>)
- Entity (&lt;)
- Element (<date>01-01-2022</date>)
- Attribute (version="1.0"/encoding="UTF-8")
- Declaration (<?xml version="1.0" encoding="UTF-8"?>)

- <         &lt;
- <         &gt;
- &        &amp;
- "          &quot;

Comments: <!-- -->


##### XML DTD (Document Type Definition)

![[Pasted image 20250209144313.png]]
Child elements declared in ()
PCDATA = raw data

DTD can be placed within the XML document itself, right after the XML Declaration in the first line. Otherwise, it can be stored in an external file (email.dtd) and then referenced within the XML document with the SYSTEM keyword
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email SYSTEM "email.dtd">
```

Also possible to reference a DTD through a URL:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email SYSTEM "http://inlanefreight.com/email.dtd">
```


##### XML Entities

XML variables in XML DTDs to allow refactoring of variables and reduce repetitive data.

<!Entity name "Value">

Once we define an entity, it can be referenced in the XML doc between an & and ; (&company;)
We can reference External XML Entities with the SYSTEM keyword, which is followed by the external entity's path:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE email [
  <!ENTITY company SYSTEM "http://localhost/company.txt">
  <!ENTITY signature SYSTEM "file:///var/www/html/signature.txt">
]>
```

(You can also use PUBLIC instead of SYSTEM)

