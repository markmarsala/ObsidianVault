**Updates and Patching

**Configuration Management
- Audit writable files and directories and any binaries set with the SUID bit.
- Ensure that any cron jobs and sudo privileges specify any binaries using the absolute path.
- Do not store credentials in cleartext in world-readable files.
- Clean up home directories and bash history.
- Ensure that low-privileged users cannot modif any custom libraries called by programs.
- Remove any unnecessary packages and services that potentially increase the attack surface.
- Consider implementing SELinux, which provides additional access controls on the system.

**User Management
- Limit number of user and admin accounts
- Ensure logon attempts are logged and monitored
- Enforce a strong password policy
- Rotate password periodically
- Restrict users from reusing old password by using the /etc/security/opasswd file with the PAM module
- Principle of least privilege

Automation tools for configuration management:
- Puppet
- SaltStack
- Zabbix (remote actions / checksum verification)
- Nagios (remediation actions)

**Audit
- STIGs - DISA Security Technical Implementation Guides
- ISO27001
- PCI-DSS
- HIPAA

Auditing tool:
- Lynis
```shell-session
./lynis audit system
```
