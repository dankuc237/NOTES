# 
1. Download Nessus package
   https://www.tenable.com/downloads/nessus?loginAttempted=true
2. Install Nessus
   ```bash
   $ sudo apt install ./Nessus-xxx-debian10_amd64.deb
   ``` 
3. Start Nessus service
   ```bash
   $ sudo systemctl start nessusd.service
   ```
4. Navigate to **https://127.0.0.1:8834**
5. Create account :`Register for Nessus Essentials`,  (eg. `kali:kali)`, ...

# Docker
1.  Download Nessus container
   ```bash
	$ sudo docker pull tenable/nessus:latest-ubuntu
   ```
2. Run container
   ```bash
   $ sudo docker run -p 8834:8834 -d tenable/nessus:latest-ubuntu
   ```


# Prepare Windows to Nessus credentialed scan
https://docs.tenable.com/nessus/Content/CredentialedChecksOnWindows.htm

https://isgovern.com/blog/how-to-setup-your-windows-environment-for-a-nessus-credentialed-patch-scan/

The steps to configure the environment are as follows:

- Create a dedicated Nessus administrator account which has full local access to Windows machines.
- Ensuring network profile is configured as ‘Private’
- Allow WMI access through the firewall
- Allow File and Print Sharing through the firewall
- Create a ‘LocalAccountTokenFilterPolicy’ registry entry
- Configure Remote Registry service


```powershell
# PROJECT

# Function to create a dedicated Nessus administrator account
function New-NessusAdmin {
    param (
        [string]$username,
        [string]$password
    )

    $securePassword = ConvertTo-SecureString $password -AsPlainText -Force
    New-LocalUser -Name $username -Password $securePassword -FullName "Nessus Administrator" -Description "Dedicated Nessus Administrator Account" -PasswordNeverExpires
    Add-LocalGroupMember -Group "Administrators" -Member $username
    Write-Host "Nessus administrator account $username created and added to Administrators group."
}

# Function to configure network profile as Private
function Set-NetworkProfilePrivate {
    Get-NetConnectionProfile | Where-Object {$_.NetworkCategory -ne 'Private'} | Set-NetConnectionProfile -NetworkCategory Private
    Write-Host "Network profile configured as 'Private'."
}

# Function to allow WMI access through the firewall
function Allow-WMIFirewall {
    Enable-NetFirewallRule -Name "WMI-RPCSS-In-TCP"
    Enable-NetFirewallRule -Name "WMI-WINMGMT-In-TCP"
    Enable-NetFirewallRule -Name "WMI-WINMGMT-Out-TCP"
    Enable-NetFirewallRule -Name "WMI-WINMGMT-In-UDP"
    Enable-NetFirewallRule -Name "WMI-WINMGMT-Out-UDP"
    Write-Host "WMI access allowed through the firewall."
}

# Function to allow File and Print Sharing through the firewall
function Allow-FilePrintSharingFirewall {
    Enable-NetFirewallRule -Group "@FirewallAPI.dll,-28502"
    Write-Host "File and Print Sharing allowed through the firewall."
}

# Function to create 'LocalAccountTokenFilterPolicy' registry entry
function Set-LocalAccountTokenFilterPolicy {
    $regPath = "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System"
    $regName = "LocalAccountTokenFilterPolicy"
    $regValue = 1

    New-ItemProperty -Path $regPath -Name $regName -Value $regValue -PropertyType DWORD -Force
    Write-Host "LocalAccountTokenFilterPolicy registry entry created."
}

# Function to configure Remote Registry service
function Configure-RemoteRegistry {
    Set-Service -Name RemoteRegistry -StartupType Automatic
    Start-Service -Name RemoteRegistry
    Write-Host "Remote Registry service configured and started."
}

# Main script execution
$username = "NessusAdmin"    # Replace with your desired username
$password = "YourPassword!"  # Replace with your desired password

New-NessusAdmin -username $username -password $password
Set-NetworkProfilePrivate
Allow-WMIFirewall
Allow-FilePrintSharingFirewall
Set-LocalAccountTokenFilterPolicy
Configure-RemoteRegistry

Write-Host "Environment configuration complete."

```