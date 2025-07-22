## Windows Defender
- It will block PowerView
- We can use the built-in PowerShell cmdlet Get-MpComputerStatus to get current Defender status (if RealTimeProtection is true, then Defender is active)

## AppLocker
Sometimes, PowerShell.exe is blocked at location %SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe
- You can call it from other locations
  -  %SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe
  -  PowerShell_ISE.exe
```shell-session
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```

## PowerShell Constrained Language Mode
- ConstrainedLagnuage blocks many useful features of PowerShell
```shell-session
$ExecutionContext.SessionState.LanguageMode
```

## LAPS
```shell-session
Find-LAPSDelegatedGroups
```
- Target AD users who can read LAPS passwords
```shell-session
Find-AdmPwdExtendedRights
```
```shell-session
Get-LAPSComputers
```
