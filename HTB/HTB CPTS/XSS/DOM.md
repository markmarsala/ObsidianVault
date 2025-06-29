#### DOM-Based XSS (Document Object Model)


No HTTP request is made. This is indicated by a hashtag (#) in the URL.

Source: the JavaScript object that takes the user input

Sink: function that writes the user input to a DOM Object on the page.

Sinks:
- document.write()
- DOM.innerHTML
- DOM.outerHTML

jQuery library functions that write to DOM objects:
- add()
- after()
- append()

We can look at the source code, check script.js to see if vulnerable. If output is not sanitized, it is vulnerable.

Payload:
- <img src="" onerror=alert(window.origin)>

To target a user, send them the URL after inputting the payload.

