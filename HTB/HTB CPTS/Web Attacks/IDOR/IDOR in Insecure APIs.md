Call APIs or execute functions as another user.
- Change another user's private information
- Reset another user's password
- Buy items using another user's payment information.

## Identifying Insecure APIs
- PUT request updates item details
- POST creates new items
- DELETE deletes items
- GET retrieve item details

In the example, a PUT request is sent with a few hidden parameters, including uid, uuid, and most interestingly role
- If we change the role, we can grant us more privileges

**Exploiting Insecure APIs
1. Change our uid to another user's uid, such that we can take over their accounts
2. Change another user's details, which may allow us to perform several web attacks
3. Create new users with arbitrary details, or delete existing users
4. Change our role to a more privileged role (e.g. admin) to be able to perform more actions
