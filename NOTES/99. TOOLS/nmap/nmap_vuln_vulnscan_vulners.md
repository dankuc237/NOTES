# vuln
Running these commands can be dangerous because of invasive and disruptive aspects of specific vulnerability scans. 
Instead of simply obtaining information, certain scans attempt to verify a vulnerability by attempting to exploit the vulnerability. 
In some cases, a successful exploitation will result in changes to the service or even crashing the service, website, or operating system.
```bash
$ sudo nmap --script vuln <target domain or IP Address> -v
$ nmap --script vuln [--script-args mincvss=6.5] <target>
```
# vulscan
user must first clone the software from the github repository
```bash
# clone repo with vulscan
$ sudo git clone https://github.com/scipag/vulscan
# add script to nmap script repository
$ sudo ln -s pwd /scipag_vulscan /usr/share/Nmap/scripts/vulscan
```
```bash
$ sudo nmap -sV --script=vulscan/vulscan.nse <target>
```
# vulners
```bash
# clone repository
$ sudo git clone https://github.com/vulnersCom/Nmap-vulners.git /usr/share/Nmap/scripts/vulners
```
```bash
$ sudo Nmap -sV --script Nmap-vulners/vulners.nse <target>
```