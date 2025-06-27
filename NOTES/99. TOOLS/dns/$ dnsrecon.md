```bash
$ dnsrecon -d <domain> [-n <DNS_IP>] [-D <subdomain_dictionary> -t brt ] [-x <XML_file>] [-t axfr]
# DNS enumeration and scaning tool with subdomain enumeration, save output to XML, perform zone transfer

$ dnsrecon -r <IP_range> # DNS reverse lookup 
```
```bash
$ dnsrecon -d megacorpone.com -D ~/subdomains.lst -t brt # bruteforce subdomains
```