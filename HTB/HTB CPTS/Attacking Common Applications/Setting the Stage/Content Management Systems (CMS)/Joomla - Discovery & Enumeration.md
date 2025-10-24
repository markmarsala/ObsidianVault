## Discovery/Footprinting

```
curl -s http://dev.inlanefreight.local/ | grep Joomla
curl -s http://dev.inlanefreight.local/README.txt | head -n 5
curl -s http://dev.inlanefreight.local/administrator/manifests/files/joomla.xml | xmllint --format -
```
- cache.xml for version is located at plugins/system/cache/cache.xml


## Enumeration
```
sudo pip3 install droopescan
droopescan scan joomla --url http://dev.inlanefreight.local/
```

JoomlaScan: https://github.com/drego85/JoomlaScan
**Alternative Installation of Python2.7 (for JoomlaScan)
```
curl https://pyenv.run | bash
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init -)"' >> ~/.bashrc
source ~/.bashrc
pyenv install 2.7
pyenv shell 2.7
python2.7 -m pip install urllib3
python2.7 -m pip install certifi
python2.7 -m pip install bs4
```

**JoomlaScan
```
python2.7 joomlascan.py -u http://dev.inlanefreight.local
```
- Joomla 3.9.4
- Administrator login is http://dev.inlanefreight.local/administrator/index.php

**Login Bruteforcing
https://github.com/ajnik/joomla-bruteforce
```
sudo python3 joomla-brute.py -u http://dev.inlanefreight.local -w /usr/share/metasploit-framework/data/wordlists/http_default_pass.txt -usr admin
```
