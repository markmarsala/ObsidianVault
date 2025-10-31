**ldapsearch
```
ldapsearch -H ldap://ldap.example.com:389 -D "cn=admin,dc=example,dc=com" -w secret123 -b "ou=people,dc=example,dc=com" "(mail=john.doe@example.com)"
```
- Bind (authenticate) as admin with password secret123
- Search under the base DN
- Use the filter mail=johndoe@example.com to find entries that have this email address

## LDAP Injection
- *
- ( )
- |
- &
- (cn=*)

Example:
```
(&(objectClass=user)(sAMAccountName=$username)(userPassword=$password))
```
- * would bypass security and select any username or any password
 

## Enumeration

**nmap
```
nmap -p- -sC -sV --open --min-rate=1000 10.129.204.229
```


Type * and * into username and password and you're in!

