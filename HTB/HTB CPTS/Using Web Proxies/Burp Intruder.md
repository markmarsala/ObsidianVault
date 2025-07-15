
Burp Intruder is a web fuzzer that can be used to fuzz pages, directories, sub-domains, parameters, parameters values, and many other things.

[CTRL+I] OR send to intruder

### Pointer:
At the beginning of request, it must be GET /\$DIRECTORY$/ HTTP/1.1
- To add the $ symbol, press 'add' 

### Attack type:
Sniper is simple since it only uses one position.

### Payloads
 - Types:
    - Simple: we give Burp a wordlist, it iterates over it
    - Runtime: loads line-by-line as the scan runs to avoice excessive memory usage by burp
    - Character substitution: lets us specify a list of characters and their replacements, and Burp Intruder tries all potential permutations

Press 'load' to choose a wordlist
You can add multiple wordlists
If the wordlist is large, it is best to select the runtime type

### Payload Processing
This is where we add rules, for instance, matching regex
To skip words that start with '.', select 'skip if matches regex'
-  Match regex: ^\\..*$

### Options
Grep - Match:
- Clear list
- Add '200 OK'
- Uncheck 'exclude HTTP headers'

