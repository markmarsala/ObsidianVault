
#### Hashcat Rules

	:         do nothing
	l         lowercase all letters
	u         uppercase all letters
	c         capital then all lowercase
	sXY       replace all X with Y
	$!        add ! at the end

Example rule list
```shell-session
:
c
so0
c so0
sa@
c sa@
c sa@ so0
$!
$! c
$! so0
$! sa@
$! c so0
$! c sa@
$! so0 sa@
$! c so0 sa@
```

Generate rule-based wordlist
```shell-session
hashcat --force password.list -r custom.rule --stdout | sort -u > mut_password.list
```

Rule lists:
```shell-session
/usr/share/hashcat/rules/
```
(best64 is most successful)


#### CeWL - srape words from website
```shell-session
cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist
```
- -d = depth of spider
- -m = minimum length of word
- --lowercase = store in lowercase
- -w = output file

