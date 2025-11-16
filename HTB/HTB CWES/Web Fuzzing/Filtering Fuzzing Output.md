**Gobuster
- s (include)
- b (exclude)
- --exclude-length

```
gobuster dir -u http://example.com/ -w wordlist.txt -s 200,301 --exclude-length 0
```
- Find directories with status codes 200 or 301, but exclude responses with a size of 0 (empty responses)


**FFUF
- mc (match mode)
- fc (filter code)
- fs (filter size)
- ms (match size)
- fw (filter out number of words in response)
- mw (match word count)
- fl (filter line)
- ml (match line count)
- mt (match time)

```
ffuf -u http://example.com/FUZZ -w wordlist.txt -mc 200 -fw 427 -ms >500
```
- Find directories with status code 200, based on the amount of words, and a response size greater than 500 bytes

```
ffuf -u http://example.com/FUZZ -w wordlist.txt -fc 404,401,302
```
- ffuf -u http://example.com/FUZZ -w wordlist.txt -mc 200 -fw 427 -ms >500

```
ffuf -u http://example.com/FUZZ.bak -w wordlist.txt -fs 0-10239 -ms 10240-102400
```
- Find backup files with the .bak extension and size between 10KB and 100KB

```
ffuf -u http://example.com/FUZZ -w wordlist.txt -mt >500
```
- Discover endpoints that take longer than 500ms to respond


**wenum
- hc (hide code)
- sc (show code)
- hl (hide length)
- sl (show length)
- hw (hide word)
- sw (show word)
- hs (hide size)
- ss (show size)
- hr (hide regex)
- sr (show regex)
- filter/hard-filter

```
wenum -w wordlist.txt --sc 200,301,302 -u https://example.com/FUZZ
```
- Show only successful requests and redirects:

```
wenum -w wordlist.txt --hc 404,400,500 -u https://example.com/FUZZ
```
- Hide responses with common error codes:

```
wenum -w wordlist.txt --sw 5-10 -u https://example.com/FUZZ
```
- Show only short error messages (responses with 5-10 words)

```
wenum -w wordlist.txt --hs 10000 -u https://example.com/FUZZ
```
- Hide large files and focus on smaller responses

```
wenum -w wordlist.txt --sr "admin\|password" -u https://example.com/FUZZ
```
- Filter for responses containing specific information


**Feroxbuster
- dont-scan
- S/--filter-regex
- W/--filter-words
- N/--filter-lines
- C/--filter-status
- --filter-similar-to
- s (status codes)

```
feroxbuster --url http://example.com -w wordlist.txt -s 200 -S 10240 -X "error" 
```
- Find directories with status code 200, excluding responses larger than 10KB or containing the word "error"


```
ffuf -u http://IP:PORT/post.php -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "y=FUZZ" -w /usr/share/seclists/Discovery/Web-Content/common.txt -v
```
