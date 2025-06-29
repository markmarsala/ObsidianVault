### NFS - Network File System
-------
TCP or UDP port 2049
Nearly the same as SMB, but for Linux and Unix only. Based on the ONC-RPC protocol exposed on TCP and UDP ports 111. It has no mechanism for authentication or authorization.

Config: /etc/exports

*nmap --script nfs*\* can detect NFS

Show available NFS shares:
*showmount -e 10.129.14.128*

Mounting NFS share:
*mkdir target-NFS*
*sudo mount -t nfs 10.129.14.128:/ ./target-NFS/ -o nolock*
*cd target-NFS*
*tree*

List Contents with Usernames & Group Names:
*ls -l mnt/nfs/*

List Contents with UIDs and GUIDs:
*ls -n mnt/nfs/*

Unmounting:
sudo umount ./target-NFS