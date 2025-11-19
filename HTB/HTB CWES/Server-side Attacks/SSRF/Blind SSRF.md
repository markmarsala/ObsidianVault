1. Confirm SSRF by pointing it to our server and seeing if there is a response
```
nc -lvnp 8000
```
- There is a response
2. Pointing it to itself displays "Date is unavailable. Please choose a different date!"

We can do the port scan and open ports display "Something went wrong!"


