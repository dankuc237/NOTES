```bash
$ nmap -sn <IP_range> # ping scan, network scan for hosts using ICMP
$ sudo nmap -sn <IP_range> # ARP scan, network scan for hosts using ARP
$ sudo nmap -sn <IP_range> [--send-ip] # ping scan, network scan for hosts using ICMP as privileged user
$ nmap -sn -PR <IP> # ARP network scan, IP=172.16.8.*
```
