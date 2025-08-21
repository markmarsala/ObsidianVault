Discretionary Access Control List (DACL) defines which security principals are granted or denied access to an object. They are made up of Access Control Entries (ACEs) that either allow or deny access and and defines the rights granted to that principle.

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

In this module, we will cover:
- ForceChangePassword: gives us the right to reset a user's password without first knowing their password
- GenericWrite: assign a user an SPN and perform a Kerberoasting attack, add ourselves or another security principle to a given group, or perform a resource-based constrained delegation attack
- AddSelf: shows security groups that a user can add themselves to
- GenericAll: modify group membership, force change a password, perform a targeted Kerberoasting attack, read LAPS password and gain local admin access

Visual: https://twitter.com/_nwodtuhs

ACL Attacks are use for:
- Lateral movement
- Privilege escalation
- Persistence

Attack scenarios:
- Abusing forgot password permissions
- Abusing group membership management
- Excessive user rights

**Note
Changing a user's password or performing other modfications within a client's AD domain may be "destructive." We must ask permissions before performing an attack and also reporting everything we do.

