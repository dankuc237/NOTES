A Python implementation of an SMB server. Allows to quickly set up shares and user accounts.
We also get a copy of the users NetNTLMv2 hash
```bash
$ impacket-smbserver <sharename> $(pwd) # create share
$ impacket-smbserver hax $(pwd) -smb2support # create share with smb2 support 
$ impacket-smbserver <sharename> $(pwd) -smb2support -user <user> -password <passwd> # create share with credentials

$ impacket-smbclient -L <IP> --no-pass # smb client for linux
cmd> net viev \\<IP> # smb client for windows 
```