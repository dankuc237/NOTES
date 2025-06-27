```bash
$ nmap -p445 --script smb-protocols <IP> # identify all the supported SMB versions
$ nmap -p445 --script smb-security-mode <IP> # identify the smb protocol security level
$ nmap -p445 --script smb-os-discovery <IP> # discover OS
$ nmap -p445 --script smb-enum-users <IP> # enumerate SMB users >> hydra
$ nmap -p445 --script smb-enum-shares,smb-ls [--script-args smbusername=<username>,smbpassword=<passwd>] <IP> # retrieve useful information about files shared on SMB volumes
$ nmap -p445 --script smb-enum-domains [--script-args smbusername=<username>,smbpassword=<passwd>] <IP> # enumerate domains on a system, along with their policies
$ nmap -p445 --script smb-enum-groups [--script-args smbusername=<username>,smbpassword=<passwd>] <IP> # list of groups and group's users from the remote Windows system
$ nmap -p445 --script smb-server-stats [--script-args smbusername=<username>,smbpassword=<passwd>] # grab the server's statistics
$ nmap -p445 --script smb-enum-services [--script-args smbusername=<username>,smbpassword=<passwd>] # list of services running on a remote Windows system
$ nmap -p445 --script smb-enum-sessions [--script-args smbusername=<username>,smbpassword=<passwd>] # Enumerates the users logged into a system either locally or through an SMB share.

$ nmap -sU -p U:137,T:139 -sS --script smb-enum-users <IP> # enumerate SMB users >> hydra
```