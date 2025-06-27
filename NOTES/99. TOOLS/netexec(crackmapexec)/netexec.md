# $ netexec 
```bash
$ netexec <protocol>

$ nxc <protocol> -L # view available modules for the specified protocol
```
## Using Kerberos
https://www.netexec.wiki/getting-started/using-kerberos
```bash
$ sudo nxc smb zoro.gold.local -k -u bonclay -p Ocotober2022
```
```bash
# pass-the-ticket
$ export KRB5CCNAME=/home/bonclay/impacket/administrator.ccache 
$ nxc smb zoro.gold.local --use-kcache
```
```bash
$ nxc ldap poudlard.wizard -k --kdcHost dc01.poudlard.wizard
```