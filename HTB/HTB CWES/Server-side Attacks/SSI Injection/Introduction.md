Server-Side Includes are supported by Apache and IIS.
File extensions: .shtml, .shtm, .stm

SSI Directives contains
- name
- parameter name
- value
```
<!--#name param1="value1" param2="value" -->
```

**printenv
```
<!--#printenv -->
```
- prints environment variables

**config
```
<!--#config errmsg="Error!" -->
```

**echo
```
<!--#echo var="DOCUMENT_NAME" var="DATE_LOCAL" -->
```
- DOCUMENT_NAME, DOCUMENT_URI, LAST_MODIFIED, DATE_LOCAL are supported


**exec
```
<!--#exec cmd="whoami" -->
```
- executes commands

**include
```
<!--#include virtual="index.html" -->
```
- inclusion of files in the web root directory
