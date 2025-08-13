**Information Disclosure
- By inputting a different uid, we can find the uuid and now change other users' details

**Modifying Other Users' Details
- Modify user's email address, then requesting a password reset link
- Placing an XSS payload in the 'about' field

## Chaining Two IDOR Vulnerabilities (IDOR Information Disclosure + IDOR Insecure Function Calls)
- We can make a script to enumerate uuid to find the admin (web_admin)
