# Useful Commands

### Upgrade shell

```bash
# On compromised host
python -c 'import pty; pty.spawn("/bin/bash")'
# ctrl-z

# While terminal session backgrounded
stty raw -echo
fg
```







