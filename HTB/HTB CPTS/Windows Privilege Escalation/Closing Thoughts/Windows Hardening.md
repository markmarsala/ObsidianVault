## Secure Clean OS Installation

https://www.microsoft.com/en-us/software-download/
This contains:
1. Any applications required for your employees' daily duties.
2. Configuration changes needed to ensure the functionality and security of the host in your environment.
3. Current major and minor updates have already been tested for your environment and demmed safe for host deployment.

## Updates and Patching

https://docs.microsoft.com/en-us/windows/deployment/update/how-windows-update-works

1. Windows Update scans host
2. Orchestrator decides on applicable updates
3. Update install is initiated
4. Updates are installed
5. Orchestrator Reboots Host to Finalize.

Managed by WSUS: https://docs.microsoft.com/en-us/windows-server/administration/windows-server-update-services/get-started/windows-server-update-services-wsus
or through Group Policy


## Configuration Management
Group Policy Management Console


## User Management

Password Policy settings in Group Policy: Computer Configuration\Windows Settings\Security Settings\Account Policies\Password Policy

- limit admin and privileged accounts
- log login attempts
- strong password policy
- MFA
- Rotate passwords
- disable password reuse


## Audit

- DISA STIGs: https://public.cyber.mil/stigs/
- Microsoft's Security Compliance Toolkit: https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-security-configuration-framework/security-compliance-toolkit-10
- ISO270001: https://www.iso.org/isoiec-27001-information-security.html
- PCI-DSS: https://www.pcisecuritystandards.org/pci_security/
- HIPAA: https://www.hhs.gov/hipaa/for-professionals/security/index.html


## Logging


## Sysmon

Sysmon is a tool built by Microsoft and included in the Sysinternals Suite that enhances the logging and event collection capability in Windows. Sysmon provides detailed info about any processes, network connections, file reads or writes, login attempts and successes, and much much more.

## Network and Host Logs

- PacketBeat: https://www.elastic.co/beats/packetbeat
- IDS/IPS


## Key Hardening Measures

- Secure boot and disk encryption with BitLocker should be enabled and in use.
- Audit writable files and directories and any binaries with the ability to launch other apps.
- Ensure that any scheduled tasks and scripts running with elevated privileges specify any binaries or executables using the absolute path.
- Do not store credentials in cleartext in world-readble files on the host or in shared drives.
- Clean up home directories and PowerShell history.
- Ensure that low-privileged users cannot modify any custom libraries called by programs.
- Remove any unnecessary packages and services that potentially increase the attack surface.
- Utilize the Device Guard and Credential Guard features built-in by Microsoft to Windows 10 and most new Server Operating Systems.
- Utilize Group Policy to enforce any configuration changes needed to company systems.
