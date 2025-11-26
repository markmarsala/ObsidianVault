Broken Object Level Authorization (BOLA) - user is authorized to interact with the vulnerable endpoint
Broken Function Level Authorizaiton (BFLA) - is is not authorized to interact with the vulnerable endpoint

CWE-200: Exposure of Sensitive Information to an Unauthorized Actor

```
htbpentester9@hackthebox.com:HTBPentester9
```

- /api/v1/authentication/customer/sign-in
- /api/v1/roles/current-user
  - No roles assigned
 
- /api/v1/products/discounts
  - This endpoint says we need the ProductDiscounts_GetAll role, but if we execute this endpoint, it is success (admin did not implement role-based access control check)
 

**Prevention
- Enforce authorization at the source-code level (verify the user's roles before processsing the request, ensuring unauthorized users are denied)
