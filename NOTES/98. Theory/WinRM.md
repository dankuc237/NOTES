# [WinRM](https://learn.microsoft.com/en-us/windows/win32/winrm/portal) (Windows Remote Management):

https://exploit-notes.hdks.org/exploit/windows/protocol/winrm-pentesting/

- **Ports:** 5985/TCP (WinRM HTTP) or 5986/TCP (WinRM HTTPS) to exchange XML messages.
- **Required Group Memberships:** Requires administrator or Remote Management Users group membership.

Windows Remote Management (WinRM) is a web-based protocol used to send Powershell commands to Windows hosts remotely. 
Most Windows Server installations will have WinRM enabled by default.


Windows Remote Management (WinRM) is the Microsoft implementation of the [WS-Management protocol](https://learn.microsoft.com/en-us/windows/win32/winrm/ws-management-protocol), which is a standard Simple Object Access Protocol (SOAP)-based, firewall-friendly protocol that allows interoperation between hardware and operating systems from different vendors.

Alternative method for remote execution is PowerShell remoting with `New-PSSession`.




```PowerShell
# connect to a remote Powershell session from the command line
cmd> winrs.exe -u:<user> -p:<passwd> -r:<target_IP> "cmd /c hostname & whoami" 
```
**Powershell**

```powershell
$username = 'Administrator';
$password = 'Mypass123';
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force; 
$credential = New-Object System.Management.Automation.PSCredential $username, $securePassword;

# create an interactive session using the Enter-PSSession cmdlet
PS> Enter-PSSession -Computername TARGET -Credential $credential

# OR runs ScriptBlocks remotely via WinRM
PS> Invoke-Command -Computername TARGET -Credential $credential -ScriptBlock {whoami} 
```
```powershell
$username = 'user.name'
$password = 'mypass' | ConvertTo-SecureString -AsPlainText -Force;
$credential = [pscredential]::new($username, $password)
$session = New-PSSession -ComputerName <target-ip> -Credential $credential
$session | Enter-PSSession

# When finished
$session | Remove-PSSession
```
