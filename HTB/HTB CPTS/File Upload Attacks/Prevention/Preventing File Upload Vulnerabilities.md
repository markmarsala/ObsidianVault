## Extension Validation
```php
$fileName = basename($_FILES["uploadFile"]["name"]);

// blacklist test
if (preg_match('/^.*\.ph(p|ps|ar|tml)/', $fileName)) {
    echo "Only images are allowed";
    die();
}

// whitelist test
if (!preg_match('/^.*\.(jpg|jpeg|png|gif)$/', $fileName)) {
    echo "Only images are allowed";
    die();
}
```

## Content Validation
```php
$fileName = basename($_FILES["uploadFile"]["name"]);
$contentType = $_FILES['uploadFile']['type'];
$MIMEtype = mime_content_type($_FILES['uploadFile']['tmp_name']);

// whitelist test
if (!preg_match('/^.*\.png$/', $fileName)) {
    echo "Only PNG images are allowed";
    die();
}

// content test
foreach (array($contentType, $MIMEtype) as $type) {
    if (!in_array($type, array('image/png'))) {
        echo "Only PNG images are allowed";
        die();
    }
}
```

## Upload Disclosure
Hide uploads directory from the end-users and only allow them to download the uploaded files through a download page.
We may write a download.php script which enforces strict authorization checks and path validation to fetch the requested file from the uploads directory and then download the file for the end-user.
Use these HTTP headers:
- Content-Disposition: Used to specify how the content should be displayed in the browser. Setting it to attachment instructs the browser to download the file rather than render it inline.
- Content-Type: Specifies the MIME type of the file, ensuring that the browser knows how to handle the file content appropriately.
- X-Content-Type-Options: nosniff: Prevents the browser from MIME-type sniffing, which helps mitigate security risks by ensuring that the browser adheres strictly to the specified Content-Type.

When storing a file in the backend, randomize its filename. This way the end user doesn't know the name.
Create an uploads server.
Use open_basedir in PHP to restrict to only web directory.

## Further Security
1. PHP: disable_functions in php.ini add dangerous functions like exec, shell_exec, system, passthru
2. Limit display of errors to be generic
3. Limit file size
4. Update any used libraries
5. Scan uploaded files for malware or malicious strings
6. Utilize a WAF
