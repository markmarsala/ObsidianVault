
## Automatic Request Modification
Example:
- Change User-Agent to something else like HackTheBox Agent 1.0, handy when dealing with filters that block certain User-Agents.

### Burp Match and Replace
Proxy > Options > Match and Replace
Type: Request header
Match: ^User-Agent.\*$
Replace: User-Agent: HackTheBox Agent 1.0
Comment:
Regex match: true

### ZAP Replacer
[CTRL+R] OR clicking on Replacer in ZAP's options menu > Add
Description: HTB User-Agent
Match Type: Request Header (will add if not present).
Match String: User-Agent
Replacement String: HackTheBox Agent 1.0.
Enable: true.
[CTRL+B] to enable interception.

### ZAP Initiator
Enable us to select where our replacer option will be applied
Keep default option of Apply to all HTTP(s) messages to apply everywhere

### Automatic Response Modification
Proxy > Options > Match and Replace
Type: Response body
Match: type="number"
Replace: type="text"
Regex match: False.
[CTRL+SHIFT+R]
