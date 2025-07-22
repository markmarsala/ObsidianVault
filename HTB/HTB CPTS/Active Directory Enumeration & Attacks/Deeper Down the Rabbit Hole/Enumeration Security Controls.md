## Windows Defender
- It will block PowerView
- We can use the built-in PowerShell cmdlet Get-MpComputerStatus to get current Defender status (if RealTimeProtection is true, then Defender is active)

## AppLocker
Sometimes, PowerShell.exe is blocked at location %SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe
- You can call it from other locations
  -  %SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe
  -  PowerShell_ISE.exe
 
