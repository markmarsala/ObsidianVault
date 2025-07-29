Files:
- .conf
- .config
- .xml
- .sh
- .bak
- bash history
- .txt
- .php

**MySQL database credentials inside WordPress config
```shell-session
grep /DB_USER\|DB_PASSWORD' wp-config.php
```

**Credentials in web root
```shell-session
find / ! -path "*/proc/*" -iname "*config*" -type f 2>/dev/null
```

**SSH Keys
```shell-session
find / ! -path "*/proc/*" -iname "*config*" -type f 2>/dev/null
```

