## SMB
-----------
TCP 137, 138, and 139
CIFS - port 445
ACLs (Access Control Lists)
Samba - Unix version which implements Common Internet File System (CIFS)
Network Basic Input/Output System (NetBIOS) API - blueprint for an application to connect and share data with other computers

List server's shares with anonymous access (-N)
*smbclient -N -L //10.129.14.128*

Connect to SMB share
*smbclient //10.129.14.128/notes -U=user*

With domain-level security, the samba server acts as a member of a Windows domain. Each domain has at least one domain controller, usually a Windows NT server providing password authentication. This domain controller provides the workgroup with a definitive password server. The domain controllers keep track of users and password in their own NTDS.dit and Security Authentication Module (SAM) and authenticate each suer when they log in for the first time and wish to access another machine's share.

rpcclient: *rpcclient -U "" 10.129.14.128*
Remote Procedure Call (RPC)
- srvinfo - server  information
- enumdomains - enumerate all domains in the network
- querydominfo - domain, server, and user infromation
- netshareenumall - enumerates all available shares
- netsharegetinfo [share] - information about a specific share
- enumdomusers - enumerate all domain users
- queryuser [RID] - information about a specific user
	- Can brute force RIDs to get user info
	- for i in $(seq 500 1100);do rpcclient -N -U "" 10.129.14.128 -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name\|user_rid\|group_rid" && echo "";done
	- Alternative Python script: samrdump.py

- rpcclient
- SMBMap: *smbmap -H 10.129.14.128*
- CrackMapExec: *crackmapexec smb 10.129.14.128 --shares -u '' -p ''*
- enum4linux-ng
