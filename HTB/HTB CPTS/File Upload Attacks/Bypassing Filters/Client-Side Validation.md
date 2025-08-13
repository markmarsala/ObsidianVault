To bypass client-side validation:
- Modify the upload request to the back-end server
- Modify front-end code through our browser's dev tools to disable any validation in place

If upload fail page never refreshes or sends any HTTP requests after selecting our file, we have complete control over client-side validations

## Back-end Request Modification
1. Using Burp, upload a normal png
2. Manipulate the request (modify filename to shell.php and modify the content to the php web shell)

## Disabling Front-end Validation
1. [CTRL+SHIFT+C] to toggle browser's Page Inspector
2. Click profile image to start upload
```html
<input type="file" name="uploadFile" id="uploadFile" onchange="checkFile(this)" accept=".jpg,.jpeg,.png">
```
3. [CTRL+SHIFT+K], then type checkFile
```javascript
function checkFile(File) {
...SNIP...
    if (extension !== 'jpg' && extension !== 'jpeg' && extension !== 'png') {
        $('#error_message').text("Only images are allowed!");
        File.form.reset();
        $("#submit").attr("disabled", true);
    ...SNIP...
    }
}
```
4. Either add 'php' in javascript, or delete 'checkFile(this)' from html
5. [CTRL+SHIFT+C], then click profile image, then we will see the URL of our uploaded web shell
6. Web shell located at /profile_images/shell.php?cmd=id
