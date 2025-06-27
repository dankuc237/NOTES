```bash
$ nmap -sU -p 161 <IP_address>
$ nmap -sU -p 161 --script snmp-* demo.ine.local | tee snmp_output # run all snmp scripts
$ nmap -sU -p 161 <IP> --script snmp-brute # bruteforce community names >> snmpwalk -c ...
$ nmap -sU -p 161 <IP> --script snmp-brute --script-args snmp-brute.communitiesdb=<wordlist># bruteforce community names with <wordlist>
$ nmap -sU -p 161 --script=snmp-win32-services <IP> # enumerate services available on target machine
$ nmap -sU -p 161 <IP> --script snmp-win32-users# available users on machine >> hydra smb:...
$ sudo nmap -sU -p 161 <IP> --script snmp-brute --script-args snmp-brute.communitiesdb=/usr/share/seclists/Misc/wordlist-common-snmp-community-strings.txt # bruteforce with seclist
```