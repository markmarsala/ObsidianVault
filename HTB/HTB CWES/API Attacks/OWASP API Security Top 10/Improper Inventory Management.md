Developers forgot to remove the v0 endpoints.

Therefore, deleted endpoints like /api/v0/customers/deleted are exposed with sensitive data.


**Prevention
Remove v0 entirely or restrict access exclusively for local development and testing purposes, ensuring it remains inaccessible to external users.
