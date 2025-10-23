**Nmap - Web Discovery
```
nmap -p 80,443,8000,8080,8180,8888,10000 --open -oA web_discovery -iL scope_list
```

**Tools:
- EyeWitness: https://github.com/FortyNorthSecurity/EyeWitness
- Aquatone: https://github.com/michenriksen/aquatone

**Reporting Notebook Structure
External Penetration Test - Client Name
- Scope
- Client Points of Contact
- Credentials
- Discovery/Enumeration
  - Scans
  - Live hosts
- Application Discovery
  - Scans
  - Interesting/Notable Hosts
- Exploitation
  - Hostname or IP
  - Hostname or IP
- Post-Exploitation
  - Hostname or IP
  - Hostname or IP
 
## Initial Enumeration
```
cat scope_list

app.inlanefreight.local
dev.inlanefreight.local
drupal-dev.inlanefreight.local
drupal-qa.inlanefreight.local
drupal-acc.inlanefreight.local
drupal.inlanefreight.local
blog-dev.inlanefreight.local
blog.inlanefreight.local
app-dev.inlanefreight.local
jenkins-dev.inlanefreight.local
jenkins.inlanefreight.local
web01.inlanefreight.local
gitlab-dev.inlanefreight.local
gitlab.inlanefreight.local
support-dev.inlanefreight.local
support.inlanefreight.local
inlanefreight.local
10.129.201.50
```
```
sudo  nmap -p 80,443,8000,8080,8180,8888,10000 --open -oA web_discovery -iL scope_list
sudo nmap --open -sV 10.129.201.50
```


## Using EyeWitness
```
sudo apt install eyewitness
eyewitness -h
eyewitness --web -x web_discovery.xml -d inlanefreight_eyewitness
```


## Using Aquatone
```
wget https://github.com/michenriksen/aquatone/releases/download/v1.7.0/aquatone_linux_amd64_1.7.0.zip
unzip aquatone_linux_amd64_1.7.0.zip
echo $PATH

/home/mrb3n/.local/bin:/snap/bin:/usr/sandbox/:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/usr/share/games:/usr/local/sbin:/usr/sbin:/sbin:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
```
- Move binary to PATH so we can call it anywhere

```
cat web_discovery.xml | ./aquatone -nmap
```


## Interpreting the Results

High Value Targets!
- Tomcat
- Custom web applications
- Content Management System: WordPress, Joomla, Drupal
- osTicket

