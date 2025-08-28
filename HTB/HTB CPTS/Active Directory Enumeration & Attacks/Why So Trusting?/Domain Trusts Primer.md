A trust allows users to access resource in another domain, outside of the domain where their account resides.

Authentication systems of two domains may allow either one-way or two-way communication.

- Parent-child: two or more domains within the same forest. Child domain has a two-way transitive trust with parent domain. Child domain corp.inlanefreight.local coud authenticate into the parent domain inlanefreight.local, and vice-versa.
- Cross-link: trust between child domains to speed up authentication.
- External: a non-transitive trust between two separate domains in separate forests which are not already joined by a forest trust.
- Tree-root: a two-way transitive trust between a forest root domain and a new tree root domain.
- Forest: a transitive trust between two forest root domains.
- ESAE: a bastion forest used to manage Active Directory

Transitive trust means if A trusts B and B trust C, then A trusts C.
Non-transitive trust means child domain is the only one trusted


## Enumerating Trust Relationships

**Using Get-ADTrust
```
Import-Module activedirectory
Get-ADTrust -Filter *
```
- ForestTransitive property is set to True which means that this is a forest trust or external trust.
- Set to bidirectional, meaning that users can authenticate back an forth across both trust.
- If we cannot authenticate across a trust, we cannot perform any enumeration or attacks across the trust.

**PowerView Find Trusts
```
Get-DomainTrust
Get-DomainTrustMapping
```

**Checking Users in the Child Domain using Get-DomainUser
```
Get-DomainUser -Domain LOGISTICS.INLANEFREIGHT.LOCAL | select SamAccountName
```

**Using netdom to query domain trust on cmd
```
netdom query /domain:inlanefreight.local trust
netdom query /domain:inlanefreight.local dc
netdom query /domain:inlanefreight.local workstation
```

BloodHound -> Map Domain trusts

