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
