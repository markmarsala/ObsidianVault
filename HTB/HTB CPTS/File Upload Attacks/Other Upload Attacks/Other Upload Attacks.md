## Injections in File Name
- file$(whoami).jpg
- file`whoami`.jpg
- file.jpg||whoami
- <script>alert(window.origin);</script>
- file';select+sleep(5);--.jpg

## Upload Directory Disclosure
- Utilize fuzzing to look for the uploads directory
- LFI/XXE to read source code
- Forcing error messages (upload a file with a name that already exists or sending two identical requests simultaneously) (upload a file with an overly long name)

## Windows-specific Attacks
- Use (|, <, >, *, or ?) in filename
- Use (CON, COM1, LPT1, or NUL) in filename
- Overwrite web.conf file with (WEB~1.CON)
