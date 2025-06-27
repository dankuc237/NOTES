# $ smbclient
```bash

$ smbclient -L <IP> # list all shares from <IP>, Anonymous login
$ smbclient -L <IP> -N # List all available shares on the SMB server, no password

$ smbclient \\\\<IP>\\<share> # connect to share with NULL session, blank username and password>password: press Enter
$ smbclient \\\\<IP>\\\\<share> -N # Anonymous login, check if share exist, connect to share without password
$ smbclient \\\\<IP>\\\\<share> -U <user> -N # check if share exist, connect to share as <user> with no password 
$ smbclient \\\\<IP>\\\\<share> -U <user>  # check if share exist, connect to share as <user> with password from terminal
$ smbclient [-U [domain/]<user> [-N/--password=<password>/[--pw-nt-hash=<NT_hash>]]] \\\\<Host>\<share> # differernt methods to access share
```