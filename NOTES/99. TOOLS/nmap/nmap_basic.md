**Report processing**
```bash
# make html page from xml report
$ xsltproc report.xml -o asd
```



# nmap port scan

```bash
$ nmap <-p -/ports/ranges> <--min-rate=1000> <IP> # TCP por scan
$ nmap <-sU> <-p-> <--min-rate=1000> <IP> # UDP port scan
```

|                   |                                                                                                                                                       |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-sS`             | domyślnie skan -niepełny 3 way handshake, stealth("nie zauważalny tak po chuju bo pakiety są puste i podejrzane, zwykły skan mniej się rzuca w oczy") |
| `-sT`             | skanowanie z pełnym 3 way handshake                                                                                                                   |
| `-sU`             | skan UDP                                                                                                                                              |
| `-p-`             | skanuj wszystkie porty                                                                                                                                |
| `--open`          | Only show open (or possibly open) ports                                                                                                               |
| `--min-rate=1000` | minimum 1000 pakietów na 1 s                                                                                                                          |
| `--top-ports 20`  | 20 najczęstrzych portów                                                                                                                               |
| `-F`              | top 100 ports                                                                                                                                       |
| `-T`              | speed of scan (0-5)                                                                                                                                  |

## $ nmap advanced port scan

```bash
$ nmap -p <found_ports> <IP> <-vv> <-sC> <-sV> # scan very verbose, default scripts, service versions
```

|                                                      |                                                                                                                                                      |
| ---------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `-vv`                                                | verbose- więcej "v", więcej opisu                                                                                                                    |
| `-sC`                                                | `--script=default`                                                                                                                                   |
| `-sV`                                                | service version                                                                                                                                      |
| `-O`                                                 | OS detection                                                                                                                                         |
| <br>`-oN`<br>`-oX`<br>`-oS`<br>`-oG`<br>`-oA <file>` | wynik skanowania zapisać w plikach o formatach<br>normal<br>XML<br>skrIpt kIddi3<br>grepable format<br>w 3 pierwszych foramtach na raz i nazwa pliku |
| `--script=  **`                                      | (folder ze skryptami /usr/share/nmap/scripts)\*\*                                                                                                    |
| `-A`                                                 | -agresive                                                                                                                                            |

# scan port ranges and hosts

```bash
$ nmap -p <21,22,80,443> <10.10.56.0/24> # scan chosen ports from IP range
$ nmap <192.168.18.10-25> # scan ports from IP range
$ nmap <192.168.*.10> # scan ports from IP range
$ nmap <localhost> # scan ports from host
$ nmap -iL <path/to/file/targets.txt> # scan ports listed in file
```

# IDS/Firewall evasion
```bash
$ nmap –sS –D <DecoyIP1>,<DecoyIP2>,<DecoyIP3>,ME <target> # IDS/Firewall evasion by decoy scan
$ nmap -D <RND:10> # IDS/Firewall evasion by decoy, scan create 10 random decoys
$ nmap -sS -f <TargetIP> # IDS/Firewall evasion by fragmentacja pakietów
$ nmap –sS –T<0~5> <target> # IDS/Firewall evasion by timing
$ nmap –sS --source-port <port> <target> # IDS/Firewall evasion by different source port
$ nmap --data-length <num> # append random data to sent packets
$ nmap -spoof-mac <0(random_MAC)/mac_address/prefix/vendor_name> # Spoof your MAC address
$ nmap --randomize-hosts -iL <list_hosts.txt> # scan hosts in random order
$ nmap -T<0-5> # scan timing, 0- very slow, 5- very fast scan
```

# Scripts
**UPDATE NMAP SCRIPT DATABASE after download new script to /usr/share/nmap/scripts**
```bash
$ sudo nmap --script-updatedb
```

## Categories

https://nmap.org/book/nse-usage.html#nse-categories

```bash
$ nmap --script=<"vuln"/"brute"/"auth"/"broadcast"/"default"/"discovery"/"dos"/"exploit"/"external"/"fuzzer"/"intrusive"/"malware"/"safe"/"version"> # nmap scripts categories
```

|             |                                                                                                                                                                                                                                                                                                |
| ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `vuln`      | check for specific known vulnerabilities and generally only report results if they are found.                                                                                                                                                                                                  |
| `brute`     | brute force attacks to guess authentication credentials of a remote server.                                                                                                                                                                                                                    |
| `auth`      | deal with authentication credentials (or bypassing them) <br>Scripts which use brute force attacks to determine credentials are placed in the brute category instead.                                                                                                                          |
| `broadcast` | Scripts in this category typically do discovery of hosts not listed on the command line by broadcasting on the local network. Use the newtargets script argument to allow these scripts to automatically add the hosts they discover to the Nmap scanning queue.                               |
| `default`   |                                                                                                                                                                                                                                                                                                |
| `discovery` | actively discover more about the network by querying public registries, SNMP-enabled devices, directory services, and the like.                                                                                                                                                                |
| `dos`       | Scripts in this category may cause a denial of service. Sometimes this is done to test vulnerability to a denial of service method, but more commonly it is an undesired by necessary side effect of testing for a traditional vulnerability. These tests sometimes crash vulnerable services. |
| `exploit`   | actively exploit some vulnerability.                                                                                                                                                                                                                                                           |
| `external`  | may send data to a third-party database or other network resource.                                                                                                                                                                                                                             |
| `fuzzer`    | scripts which are designed to send server software unexpected or randomized fields in each packet.                                                                                                                                                                                             |
| `intrusive` | scripts that cannot be classified in the safe category,crash the target system, use up significant resources on the target host (such as bandwidth or CPU time), or otherwise be perceived as malicious by the target's system administrators.                                                 |
| `malware`   | test whether the target platform is infected by malware or backdoors.                                                                                                                                                                                                                          |
| `safe`      | Scripts which weren't designed to crash services                                                                                                                                                                                                                                               |
| `version`   | extension to the version detection feature<br>run only if version detection (-sV) was requested<br>do not produce service or host script results.                                                                                                                                              |

## nmap --script


https://nmap.org/nsedoc/scripts/

```bash
$ nmap --script-help <script_name>
$ nmap <target> --script=<script_name> # np.: --script=http-fileupload-exploiter
$ nmap --script="(default or safe or intrusive) and not http-*" # kombinacja różnych skryptów
```

```bash
$ nmap --script=<vuln/vulners/vulscan/broadcast-ping/ipidseq/targets-sniffer> -e <eth0> # usefull nmap scripts
```

|                          |                                                                                                                                                                                                                                                                                                                                        |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `vuln`                   | uses its own internal database that is periodically updated with new vulnerability rules and signatures, wewnętrzna baza nmap                                                                                                                                                                                                          |
| `vulners`                | `[--script-args mincvss=<arg_val>]` <br>sends CPE descriptions of discovered services to the vulners.com vulnerability database API and reports known CVEs in those services.<br>                                                                                                                                                      |
| `vulscan`                |Required download<br>https://github.com/scipag/vulscan The vulscan script is not included with Nmap. It does a similar lookup to the vulners script, but uses an offline copy of VulDB and some other databases. The same caveats apply, except that it does not send CPE information to an external service. |
| `broadcast-ping`         | Sends broadcast pings on a selected interface using raw ethernet packets and outputs the responding hosts' IP and MAC addresses or (if requested) adds them as targets [`--script-args=newtargets` to add the results as targets].<br>`nmap -e eth0--script=broadcast-ping [--script-args=newtargets]`                               |
| `targets-sniffer -e th0` | Sniffs the local network for a configurable amount of time (10 seconds by default) and prints discovered addresses. If the newtargets script argument is set, discovered addresses are added to the scan queue. <br> `nmap -sL --script=targets-sniffer --script-args=newtargets`                                                      |
|`ipidseq`|script is used to identify a good zombie candidate for Idle Scan|
## http

|                    |                                                                             |
| ------------------ | --------------------------------------------------------------------------- |
| `http-methods`     |                                                                             |
| `http-methods`     | `--script-args http-methods.url-path=/webdav/`                              |
| `http-enum`        | Enumerates directories used by popular web applications and servers.        |
| `http-put`         | `--script-args http-put.url='/dav/shell.php',http-put.file='./shell.php'`   |
| `http-headers`     |                                                                             |
| `http-webdav-scan` |                                                                             |
| `http-grep`        |                                                                             |
| `http-exif-spider` | extract interesting EXIF data from photos found on websites                 |
| `http-rfi-spider`  | Crawls webservers in search of RFI (remote file inclusion) vulnerabilities. |


## mysql

|                     |                                                                                                                                                                                                             |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `mysql-info`        | Connects to a MySQL server and prints information such as the protocol and version numbers, thread ID, status, capabilities, and the password salt                                                          |
| `mysql-users`       | Enumerate the users present on MySQL database server <br>`--script-args="mysqluser='root',mysqlpass=''`                                                                                                     |
| `mysql-databases`   | List all databases stored on the MySQL Server<br>`--script-args="mysqluser='root',mysqlpass=''`                                                                                                             |
| `mysql-variables`   | Find the data directory used by mysql server<br>`--script-args="mysqluser='root',mysqlpass=''"`                                                                                                             |
| `mysql-audit`       | Check whether File Privileges can be granted to non admin users<br>`--script-args "mysql-audit.username='root',mysql-audit.password='',mysql-audit.filename='/usr/share/nmap/nselib/data/mysql-cis.audit'"` |
| `mysql-dump-hashes` | Dump all user hashes<br>`--script-args="username='root',password=''"`                                                                                                                                       |
| `mysql-query`       | number of records stored in table “authors” in database “books” stored on MySQL Server<br>`--script-args="query='select count(*) from books.authors;',username='root',password=''"`                         |

## mssql

|                         |                                                                                                                                                     |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ms-sql-info`           |                                                                                                                                                     |
| `ms-sql-ntlm-info`      | `--script-args mssql.instance-port=1433`                                                                                                            |
| `ms-sql-brute`          | `--script-args userdb=users,passdb=pass`                                                                                                            |
| `ms-sql-empty-password` | check if sa user is enabled without any password                                                                                                    |
| `ms-sql-query`          | Extracting sysusers from MSSQL<br>`--script-args mssql.username=admin,mssql.password=anamaria,ms-sql-query.query="SELECT * FROM master..syslogins"` |
| `ms-sql-dump-hashes`    | Dump the hashes of MSSQL users<br>`--script-args mssql.username=admin,mssql.password=anamaria`                                                      |
| `ms-sql-xp-cmdshell`    | Execute a command using xp_cmdshell<br>`--script-args mssql.username=admin,mssql.password=anamaria,ms-sql-xp-cmdshell.cmd="type c:\flag.txt"`       |
## snmp
```bash
$ nmap -sU -p 161 <IP_address>
$ nmap -sU -p 161 --script snmp-* demo.ine.local | tee snmp_output # run all snmp scripts
$ nmap -sU -p 161 <IP> --script snmp-brute # bruteforce community names >> snmpwalk -c ...
$ nmap -sU -p 161 <IP> --script snmp-brute --script-args snmp-brute.communitiesdb=<wordlist># bruteforce community names with <wordlist>
$ nmap -sU -p 161 --script=snmp-win32-services <IP> # enumerate services available on target machine
$ nmap -sU -p 161 <IP> --script snmp-win32-users# available users on machine >> hydra smb:...
$ sudo nmap -sU -p <IP> --script snmp-brute --script-args snmp-brute.communitiesdb=/usr/share/seclists/Misc/wordlist-common-snmp-community-strings.txt # bruteforce with seclist
```
## full list

https://nmap.org/nsedoc/  
Skrypty przechowywane na komputerze `/usr/share/nmap/scripts`

There are two ways to search for installed scripts. One is by using the /usr/share/nmap/scripts/script.db
