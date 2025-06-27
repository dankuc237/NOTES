```bash
# w sesji meterpretera
post/multi/manage/autoroute # automatycznie dodaj wszystkie sieci dostepne na hoście

route # route table entries within Metasploit’s routing table
route remove 172.19.176.0/20 1 # remove a specific route<br>`route remove <IP ADDRESS OF SUBNET><NETMASK IN SLASH FORMAT> <GATEWAY>`
route flush # flush all routes from the routing table
route get 169.254.204.110 # check session the route will use

run autoroute -s 10.1.1.0 -n 255.255.255.0` # Add a route to 10.10.10.1/255.255.255.0
run autoroute -s 10.10.10.1 # Netmask defaults to 255.255.255.0
run autoroute -s 10.10.10.1/24 # CIDR notation is also okay
run autoroute -p # Print active routing table
run autoroute -d -s 10.10.10.1 # Deletes the 10.10.10.1/255.255.255.0 route
```
po pivotingu należy przeskanować nową sieć
https://www.giac.org/paper/gpen/3579/post-exploitation-metasploit-pivot-port/121704
`arp_scanner`
## SOCKS Proxy
```bash
$ cat /etc/proxychains4.conf # configure SOCKS4 proxy 
# make sure there is line socks4 127.0.0.1 9050

# run SOCKS proxy server
msf> use auxiliary/server/socks_proxy
msf> set SRVPORT 9050
msf> set VERSION 4a 
msf> exploit
msf> jobs

$ proxychains nmap ... # use proxychains
```
