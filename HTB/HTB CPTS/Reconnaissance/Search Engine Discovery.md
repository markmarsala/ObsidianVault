##### Search Engine Discovery

Operators:
- site:
- inurl:
- filetype:
- intitle:
- intext: or inbody:
- cache:
- link:
- related:
- info:
- define:
- numrange:
- allintext:
- allinurl:
- allintitle:
- AND
- OR
- NOT
- \* (wildcard search)
- .. (range search)
- "" (exact phrases)
- - (exclude)

###### Google Dorking
- Finding login pages:
	- site:example.com inurl:login
	- site:example.com (inurl:login OR inurl:admin)
- Identifying exposed files:
	- site:example.com filetype:pdf
	- site:exapmle.com (filetype:xls OR filetype:docx)
- Uncovering configuration files:
	- site:example.com inurl:config.php
	- site:example.com (ext:conf OR ext:cnf)
- Locating database backups:
	- site:example.com inurl:backup
	- site:example.com filetype:sql

##### Wayback Machine