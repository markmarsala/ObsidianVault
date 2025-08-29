Tools:
- AD Explorer: https://docs.microsoft.com/en-us/sysinternals/downloads/adexplorer
  - Takes snapshot to compare before and after engagement. Used offline for analysis.
- PingCastle: https://www.pingcastle.com/documentation/
```
pingcastle.exe
```
  - healthcheck is a baseline overview.
  - vulnereability susceptibility, our shares, trusts, the delegation of permissions, user and computer states.
- Group3r: https://github.com/Group3r/Group3r
  - Audits GPO
```
group3r.exe -f <filepath-name.log> 
```
- ADRecon: https://github.com/adrecon/ADRecon
```
.\ADRecon.ps1
```
- GroupPolicy PowerShell module should be installed.
- '-GenExcel' flag will generate an excel doc
