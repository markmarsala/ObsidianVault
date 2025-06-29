
External penetration test footholds
- Web application attacks
	- File upload attacks
	- SQL injection
	- RFI/LFI
	- Command Injection
- Password spraying
	- Against RDS
	- VPN portals
	- Citrix
	- OWA
	- Active Directory authentication
- Social Engineering

Web shells are unstable. File uploads may be deleted after a certain amount of time.

Problems:
- Web applications sometimes automatically delete files after a pre-defined period
- Limited interactivity with the operating system in terms of navigating the file system, downloading and uploading files, chaining commands together may not work (ex. `whoami && hostname`), slowing progress, especially when performing enumeration -Potential instability through a non-interactive web shell
- Greater chance of leaving behind proof that we were successful in our attack