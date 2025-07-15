
```shell-session
ls /usr/share/metasploit-framework/plugins
```

```shell-session
load nessus
```
```shell-session
nessus_help
```

If found a plugin online, place it in the directory above and give it proper permissions

```shell-session
git clone https://github.com/darkoperator/Metasploit-Plugins
```
```shell-session
sudo cp ./Metasploit-Plugins/pentest.rb /usr/share/metasploit-framework/plugins/pentest.rb
```

```shell-session
load pentest
```

- nMap (pre-installed)	
- NexPose (pre-installed)	
- Nessus (pre-installed)
- Mimikatz (pre-installed V.1)	
- Stdapi (pre-installed)	
- Railgun
- Priv	
- Incognito (pre-installed)	
- Darkoperator's

