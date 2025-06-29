### FTP
----
TCP Port 21 - control channel
TCP Port 20 - data channel

*Active FTP*: client tells server which port to send data too; may be blocked by firewall
*Passive FTP*: server tell client which port for data channel; since the client initiates the connection, the firewall does not block the transfer.

Authentication usually needed 

anonymous FTP - allows public to upload and download files without a password

TFTP - Trivial File Transfer Protocol
UDP Port 21, no authentication
Global read and write

Commands:
- connect
- get - remote to local
- put - local to remote
- quit
- status
- verbose
- debug
- trace

**vsFTPd**
config: /etc/vsftpd.conf

/etc/ftpusers - denies certain users access to the FTP service

Download all available files
*wget -m --no-passive ftp://anonymous:anonymous@10.129.14.136


find all ftp scripts on your host: 
*find / -type f -name ftp\** *2>/dev/null | grep scripts

If FTP is configured to use SSL certificate:
*openssl s_client -connect 10.129.14.136:21 -starttls ftp*