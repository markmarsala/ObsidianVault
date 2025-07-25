 Discretionary Access Control List (DACL) are made up of Access Control Entries (ACEs) and defines the rights granted to that principle.

 System Access Control Lists (SACL) allow administrators to log access attempts made to secured objects.

 Active Directory Users and Computers, select user, Permissions is DACL which contain a bunch of ACEs. SACLs are within the auditing tab.

 - Access denied
 - Access allowed
 - System audit ACE

ACE is made up of:
- The security identifier (SID) of the user/group that has access to the object (or principal name graphically)
- A flag denoting the type of ACE (access denied, allowed, or system audit ACE)
- A set of flags that specify whether or not child containers/objects can inherit the given ACE entry from the primary or parent object
- An access mask which is a 32-bit value that defines the rights granted to an object

What can be enumerated:
- ForceChangePassword abused with Set-DomainUserPassword
- Add Members abused with Add-DomainGroupMember
- GenericAll abused with Set-DomainUserPassword or Add-DomainGroupMember
- GenericWrite abused with Set-DomainObject
- WriteOwner abused with Set-DomainObjectOwner
- WriteDACL abused with Add-DomainObjectACL
- AllExtendedRights abused with Set-DomainUserPassword or Add-DomainGroupMember
- Addself abused with Add-DomainGroupMember

