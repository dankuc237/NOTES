# usefull queries
Custom Queries: https://gist.github.com/seajaysec/a4d4a545047a51053d52cba567f78a9b

All Users in domain
```SQL
MATCH (m:User) RETURN m
```
All computers in domain
```SQL
MATCH (m:Computers) RETURN m
```
Groups to which user belongs to
```SQL
MATCH (u:User {name:"R.ANDREWS@OSCP.EXAM"})-[r:MemberOf]->(andrewsGroups)  RETURN andrewsGroups
```
# nxc Bloodhound Ingestor
```bash
$ nxc ldap <ip> -u user -p pass --bloodhound --collection All
$ nxc ldap <ip> -u user -p pass --bloodhound --collection Method1,Method2
```
```bash
$ nxc ldap 192.168.195.97 -u charlotte -p "Game2On4.\!" --bloodhound --collection All --dns-server 192.168.195.97
```
# BloodHound/Sharphound
Bloodhound is the GUI that allows us to import data captured by Sharphound and visualise it into attack paths.

Sharphound is the enumeration tool of Bloodhound

[SharpHound LEGACY to use with KALI](https://github.com/SpecterOps/BloodHound-Legacy/tree/master/Collectors)

[New Sharphound support and download](https://support.bloodhoundenterprise.io/hc/en-us/categories/9270370014875-Data-Collection)
[New SharpHound releases (github)](https://github.com/SpecterOps/SharpHound/releases)

```bash
# START BLOODHOUND

$ sudo neo4j console # start neo4j
$ sudo systemctl start neo4j # start neo4j

# change the default credentials for neo4j
# go to http://localhost:7474/ and login with (neo4j:neo4j)

$ bloodhound --no-sandbox # run blodhound
```
```powershell
# RUN SHARPHOUND.PS1

# for collecting data
PS> Import-Module .\Sharphound.ps1
PS> Invoke-BloodHound -CollectionMethod All -OutputDirectory C:\Users\ -OutputPrefix "BloodHound_out"
```
```powershell
# RUN SHARPHOUND.EXE

PS> SharpHound.exe --CollectionMethods All --Domain za.tryhackme.com --ExcludeDCs

https://bloodhound.readthedocs.io/en/latest/data-collection/sharphound-all-flags.html
PS> Sharphound.exe --CollectionMethods <Methods> --Domain za.tryhackme.com --ExcludeDCs
# CollectionMethods - Determines what kind of data Sharphound would collect.
# ExcludeDCs - not to touch domain controllers, which reduces the likelihood that the Sharphound run will raise an alert.
```

There are three different Sharphound collectors:
- **Sharphound.ps1** - PowerShell script for running Sharphound.
- **Sharphound.exe** - A Windows executable version for running Sharphound.
- **AzureHound.ps1** - PowerShell script for running Sharphound for Azure.


## reset Blodhound passwd
1. Stop neo4j if its running
2. edit `/etc/neo4j/neo4j.conf`, and uncomment `dbms.security.auth_enabled=false`
3. Start neo4j
4. connect to the database and run `ALTER USER neo4j SET PASSWORD 'newpasswd'; :exit`
5. Stop neo4j
6. comment out the `dbms.security.auth_enabled=false`
7. start neo4j