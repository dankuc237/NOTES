

```bash
# Interactive
$ nslookup
>> server 192.214.31.3 # adres IP servera DNS
# >> set q=NS # tylko NS records
# >> set q=MX # tlko servery mail
>> <domain>/<IP> # domain to lookup or IP for reverse lookup
>> exit
```
```bash
$ nslookup <domain> # ask dns for only A records, return IP of server
$ nslookup <IP> <DNS_IP> # reverse dns lookup
$ nslookup [<-query=mx/ns/any>] <domain> # ask dns for list of mail servers/nameservers/wszystko
```