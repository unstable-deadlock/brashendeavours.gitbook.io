# Compilation Reference

### Install 32-bit libraries on Kali 64

```text
apt install -y gcc-multilib
```

### Compiling 32-bit code for an i686 processor

```text
gcc -Wall -o linux-sendpage linux-sendpage.c -m32 -mtune=i686 -Wl,--hash-style=both
```

