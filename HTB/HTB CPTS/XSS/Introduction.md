### Cross-Site Scripting (XSS)

- Have target user send their session cookie to the attacker's web server
- Have target's browser execute API calls, like change a user's password to a password of the attacker's choosing
- Bitcoin mining
- Displaying ads

Exploiting a binary vulnerability = system level code execution

Types of XSS:
- Stored (Persistent) XSS
	- User input is stored on the back-end database and then displayed upon arrival
- Reflected (Non-Persistent) XSS
	- User input is displayed on the page after being processed by the backend server, but without being stored
- DOM-based XSS
	- User input is directly shown in the browser and processed on the client-side, without reaching the back-end server (client-side HTTP parameters or anchor tags)
