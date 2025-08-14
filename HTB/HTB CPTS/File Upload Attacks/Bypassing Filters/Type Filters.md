Testing extensions is not enough, so modern web applications test the content of the uploaded file to ensure it matches the specified type.

Two common methods for validating the file content:
1. Content-Type Header
2. File Content

To check, upload shell.jpg with php code inside, and it fails.

## Content-Type
Example of Content-Type header validation:
```php
$type = $_FILES['uploadFile']['type'];

if (!in_array($type, array('image/jpg', 'image/jpeg', 'image/png', 'image/gif'))) {
    echo "Only images are allowed";
    die();
}
```

To bypass:
1. Fuzz Content-Type header with Burp Intruder
```shell-session
wget https://raw.githubusercontent.com/danielmiessler/SecLists/refs/heads/master/Discovery/Web-Content/web-all-content-types.txt
cat web-all-content-types.txt | grep 'image/' > image-content-types.txt
```
2. Change the Content-Type in the header of the request to image/jpg

Note: A file upload HTTP request has two Content-Type headers, one for the attached file (at the bottom), and one for the full request (at the top). We usually need to modify the file's Content-Type header, but in some cases the request will only contain the main Content-Type header (e.g. if the uploaded content was sent as POST data), in which case we will need to modify the main Content-Type header.

## MIME-Type
Multipurpose Internet Mail Extensions - determines type of a file through its general format and bytes structure
Checks magic bytes at the beginning. For example, gif is GIF8, so as long as we have this at the beginning of file, it would be considered a GIF.

Example of MIME-Type validation:
```php
$type = mime_content_type($_FILES['uploadFile']['tmp_name']);

if (!in_array($type, array('image/jpg', 'image/jpeg', 'image/png', 'image/gif'))) {
    echo "Only images are allowed";
    die();
}
```

Ty bypass:
1. Intercept request and put GIF8 before the php code
