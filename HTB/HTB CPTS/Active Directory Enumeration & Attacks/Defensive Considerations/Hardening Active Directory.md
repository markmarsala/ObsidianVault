## Step One: Document and Audit

**Things To Document and Track
- Naming conventions of OUs, computers, users, groups
- DNS, network, and DHCP configurations
- An intimate understanding of all GPOs and the objects that they are applied to
- Assignment of FSMO roles
- Full and current application inventory
- A list of all enterprise hosts and their location
- Any trust relationships we have with other domains or outside entities
- Users who have elevated permissions

## People, Processes, and Technology

**People
- Strong password policy with a password filter that disallows the use of common words.
- Rotate passwords periodically for all service accounts.
- Disallow local administrator access on user workstations unless a specific business need exists.
- Disable the default RID-500 local admin account and create a new admin account for administration subject to LAPS password rotation.
- Implement split tiers of administration for administrative users. Too often, during an assessment, you will gain access to Domain Administrator credentials on a computer tha an administrator uses for all work activities.
- Clean up privileged groups. Restrict group membership in highly privileged groups to only those users who require this access to perform their day-to-day system administrator duties.
- Where appropriate, place accounts in the Protected Users group.
- Disable Kerberos delegation for administrative accounts (the Protected Users group may not do this).

**Protected Users Group
- Prevents user credentials from being abused if left in memory on a host
```
Get-ADGroup -Identity "Protected Users" -Properties Name,Description,Members
```
- This may cause unforeseen issues with authentication, which can result in account lockouts, so it should be implemented with staged testing.


**Processes
- Proper policies and procedures for AD asset management.
  - AD host audit, the use of asset tags, and periodic asset inventories can help ensure hosts are not lost.
- Access control policies (user acount provisioning/de-provisioning), multi-factor authentication mechanisms.
- Processes for provisioning and decommissioning hosts (i.e., baseline security hardening guideline, gold images)
- AD cleanup policies
  - Are accounts for former employees removed or just disabled?
  - What is the process for removing stale records from AD?
  - Processes for decommissioning legacy operating systems/services (i.e., proper uninstallation of Exchange when migrating to O365).
  - Schedule for User, groups, and hosts audit.
 
**Technology
- Run tools such as BloodHound, PingCastle, and Grouper periodically to identify AD misconfigurations.
- Ensure that administrators are not storing passwords in the AD account description field.
- Review SYSVOL for scripts containing passwords and other sensitive data.
- Avoid the use of "normal" service accounts, utilizing Group Managed (gMSA) and Managed Service Accounts (MSA) where ever possible to mitigate the risk of Kerberoasting.
- Disable unconstrained delegation wherever possible.
- Prevent direct access to Domain Controllers through the use of hardened jump hosts.
- Consider setting the ms-DS-MachineAccountQuota attribute to 0, which disallows suers from adding machine accounts and can prevent several attack such as the noPac attack and Resource-Based Constrained Delegation (RBCD)
- Disable the print spooler service wherever possible to prevent several attacks
- Disable NTLM authentication for Domain Controllers if possible
- Use Extended Protection for Authentication along with enabling Require SSL only to allow HTTPS connections for the Certificate Authority Web Enrollment and Certificate Enrollment Web Service services
- Enable SMB signing and LDAP signing
- Take steps to prevent enumeration with tools like BloodHound
- Ideally, perform quarterly penetration tests/AD security assessments, but if budget constraints exist, these should be performed annually at the very least.
- Test backups for validity and review/practice disaster recovery plans.
- Enable the restriction of anonymous access and prevent null session enumeration by setting the RestrictNullSessAccess registry key to 1 to restrict null session access to unauthenticated users.


## Protections By Section
https://attack.mitre.org/tactics/enterprise/
- External Reconnaissance
- Internal Reconnaissance
- Poisoning
- Password Spraying
- Credentialed Enumeration
- LOTL
- Kerberoasting

