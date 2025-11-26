CWE-213: Exposure of Sensitive Information Due to Incompatible Policies.

```
htbpentester4@hackthebox.com:HTBPentester4
```
- /api/v1/authentication/customers/sign-in
- /api/v1/roles/current-user
  - Suppliers_Get
  - Suppliers_GetAll
 
/api/v1/suppliers GET shows phone numbers and emails, which is sensitive for a marketplace since a customer can contact them directly.

**Prevention 
Implement a dedicated request Data Transfer Object (DTO): https://en.wikipedia.org/wiki/Data_transfer_object


## Improperly Controlled Modification of Dynamically-Determined Object Attributes

CWE-915

```
htbpentester6@pentestercompany.com:HTBPentester6
```
- /api/v1/authentication/suppliers/sign-in
- /api/v1/roles/current-user
  - SupplierCompanies_Update
  - SupplierCompanies_Get
 
/api/v1/supplier-companies PATCH
- allows sending a value for the isExemptedFromMarketplaceFee
- can change 0 to 1, therefore the company bypasses the marketplace fee

**Prevention 
Implement a dedicated request Data Transfer Object (DTO): https://en.wikipedia.org/wiki/Data_transfer_object
