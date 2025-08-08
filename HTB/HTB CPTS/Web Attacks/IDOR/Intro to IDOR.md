Insecure Direct Object References

When a web app exploses a direct reference to an objet, like a file or a database resource, which the end-user can directly control to obtain access to other similar objects.
- Due to lack of a solid access control system

For instance, if we wanted to access a file we uploaded (download.php?file_id=123), then we can check for other file_ids, by visiting download.php?file_id=124.

IDOR exists due to lack of an access contol on the back-end.
Solution: Role-Based Access Control (RBAC)

All users could have access to all data, which is a vulnerability.

Can lead to 
- Information Disclosure
- Elevation of user privileges
- Complete web application takeover
