
Windows # of vulnerabilities: https://www.cvedetails.com/vendor/26/Microsoft.html
- MS08-067
- Eternal Blue
- PrintNightmare
- BlueKeep
- Sigred
- SeriousSam
- Zerologon

### Enumerating Windows & Fingerprinting Methods

1. TTL counter when utilizing ICMP is typically either 32 or 128, most likely 128.

TTL for each OS: https://subinsb.com/default-device-ttl-values/

2. Nmap
```shell-session
sudo nmap -v -O 192.168.86.39
```

Find banners for version of services
```shell-session
sudo nmap -v 192.168.86.39 --script banner.nse
```


### Executables
- DLLs
	- Dynamic Linking Library
	- Elevate our privileges to SYSTEM
	- Bypass User Account Controls
- Batch files (bat)
	- Run commands in an automated fashion
	- Open a port
	- Connect back to our attacking box
- VBS
	- VBScript
	- Have users perform an action such as enabling the loading of Macros in an excel document
	- Clicking on a cell to have the Windows scripting engine execute a piece of code
- MSI
	- Installation database for the Windows Installer
	- Run 'msiexec' to execute our file 
- Powershell
	- Microsoft's modern shell environment and scripting language
	- Takes input and output as .NET objects

#### Payload Generation

MSFVenom & Metasploit

PayloadsAllTheThings

Mythic C2 Framework: https://github.com/its-a-feature/Mythic

Nishang (PowerShell implants and scripts): https://github.com/samratashok/nishang

Darkarmour (obfuscated binaries for Windows): https://github.com/bats3c/darkarmour


#### Payload Transfer and Execution

- Impacket (psexec, smbclient, wmi, Kerberos, stand up an SMB server)
- Payloads All The Things
- SMB
- Remote execution via MSF
- Other Protocols (FTP, TFTP, HTTP/S)


### Example

1. Enumerate the Host
	1. Use Ping, Netcat, Nmap scans, and even Metasploit
2. Search for and decide on an exploit path
	1. EternalBlue? auxiliary/scanner/smb/smb_ms17_010
3. Select Exploit & Payload, then Deliver
4. Execute Attack, and Receive A Callback
5. Identify the Native Shell. (shell)


#### CMD vs. PowerShell

Use CMD when:
- You are on an older host that may not include PowerShell
- When you only require simple interactions/access to the host.
- When you plan to use simple batch files, net commands, or MS-DOS native tools.
- When you believe that execution policies may affect your ability to run scripts or other actions on the host.

Use PowerShell when:
- You are planning to utilize cmdlets or other custom-built scripts.
- When you wish to interact with .NET objects instead of text output
- When being stealthy is of lesser concern
- If you are planning to interact with cloud-based services and hosts
- If your scripts set and use aliases


