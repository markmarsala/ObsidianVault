## Discovery/Footprinting

Find:
- WordPress version
- Installed theme
- Plugins
- Plugin versions that are vulnerable
- User enumeration?
- Directory listing ?
- XML-RPC enabled ?

```
/robots.txt
/wp-login.php
/wp-admin.php
/wp-content.php
```

**Plugins
- wp-content/plugins

**Themes
- wp-content/themes

## Enumeration
```
curl -s http://blog.inlanefreight.local | grep WordPress
curl -s http://blog.inlanefreight.local/ | grep themes
curl -s http://blog.inlanefreight.local/ | grep plugins
curl -s http://blog.inlanefreight.local/?p=1 | grep plugins
```
- Visit plugins pages to find version and check for vulnerabilities


## Enumerating Users
/wp-login.php

Valid username and invalid password results in: "The password you entered for the username admin is incorrect."
Invalid username will say that it is not found in the database.


## WPScan
```
sudo gem install wpscan
sudo wpscan --url http://blog.inlanefreight.local --enumerate --api-token dEOFB<SNIP>
```
- https://wpvulndb.com/
- Create account and the api token will appear
