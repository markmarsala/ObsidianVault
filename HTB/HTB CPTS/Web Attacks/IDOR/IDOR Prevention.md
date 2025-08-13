**Object-Level Access Control
- Map RBAC to all objects and resources

For example:
```javascript
match /api/profile/{userId} {
    allow read, write: if user.isAuth == true
    && (user.uid == userId || user.roles == 'admin');
}
```
- web application may compare user roles to objects to allow or deny access control

**Object Referencing
- Use salted hashes or UUIDs
Example:
```php
$uid = intval($_REQUEST['uid']);
$query = "SELECT url FROM documents where uid=" . $uid;
$result = mysqli_query($conn, $query);
$row = mysqli_fetch_array($result));
echo "<a href='" . $row['url'] . "' target='_blank'></a>";
```

